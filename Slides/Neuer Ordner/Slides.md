---
output:
  beamer_presentation:
    keep_md: true
    keep_tex: no
    latex_engine: xelatex
header-includes:
#enable line breaks in chunks
  - |
    \usepackage{fvextra}
    \DefineVerbatimEnvironment{Highlighting}{Verbatim}{breaklines,commandchars=\\\{\}}
#   \DefineVerbatimEnvironment{verbatim}{Verbatim}{breaklines,commandchars=\\\{\}}
---



# Sub-problem 1

```r
if(!require("RSelenium")) install.packages("RSelenium")
library(RSelenium)
if(!require("rvest")) install.packages("rvest")
library(rvest) #for read_html(), html_elements()...
#Free all ports
  system("taskkill /im java.exe /f", intern=FALSE, ignore.stdout=FALSE)
#Start a selenium & Assign client to an R-object  
  rD <- rsDriver(port = 4561L, browser = "firefox")
  remDr <- rD[["client"]]
  #remDr$quit
```

# Define savepage()

```r
#Load url & return content as r-object 
  savepage <- function(url){
    #Navigate to starting page
      remDr$navigate(url)
    #Wait until page is loaded  
      Sys.sleep(abs(rnorm(1, 2, 1)))
    #Save content to an R-object
      remDr$getPageSource(header = TRUE)[[1]] %>%  
        read_html() %>% 
        return()
  }
```
*Note: [[1]] behinde getPageSource() unlist the output -> makes it searchable*

# Sub-problem 2

```r
#navigate to url & save content as r-object
page <- savepage("https://www.bundestag.de/webarchiv/Ausschuesse/ausschuesse19/a07/Anhoerungen")
```

# Sub-problem 3

```r
page
```

```
## {html_document}
## <html xml:lang="de" dir="ltr" class="detection-firefox" lang="de">
## [1] <head>\n<meta http-equiv="Content-Type" content="text/html; charset=UTF-8 ...
## [2] <body class="bt-archived-page">\n  <div class="bt-archive-banner">\n    < ...
```

# Sub-problem 4

```r
html_element(page, xpath= "head/meta[@property='og:image']")
```

```
## {html_node}
## <meta property="og:image" content="https://www.bundestag.dehttps://www.bundestag.de/resource/image/244626/2x3/316/475/b16fd9b7ae2fc1b1e097c016394bdcb6/ie/default.jpg">
```
