---
title: |
       ![](Slides_files/RUB.jpg){width=2.5in}
subtitle:  "Workshop: Data Wrangling of Web Data in R"
author: "Simon Ress | Ruhr-Universität Bochum"
#institute: "Conference: 56. Jahrestagung der DGSMP, Leipzig, 2021"
date: "October 18, 2021"

output:
  beamer_presentation:
    keep_md: true
    keep_tex: no
    latex_engine: xelatex
    #theme: metropolis
    slide_level: 3 # which header level should be printed as slides
    incremental: no
header-includes:
#Choose numbering format
  - \usetheme[numbering=fraction]{metropolis}
#Define footer:
  - \definecolor{beaublue}{rgb}{0.74, 0.83, 0.9}
  - \setbeamertemplate{frame footer}{\tiny{\textcolor{beaublue}{Workshop - Data Wrangling of Web Data in R | SIMON RESS}}}
#hide footer on title page:
  - |
    \makeatletter
    \def\ps@titlepage{%
      \setbeamertemplate{footline}{}
    }
    \addtobeamertemplate{title page}{\thispagestyle{titlepage}}{}
    \makeatother
#show footer on section's start/title pages:
  #overwrite "plain,c,noframenumbering" in section pages definition -> enables footer:
  - |
    \makeatletter
    \renewcommand{\metropolis@enablesectionpage}{
      \AtBeginSection{
        \ifbeamer@inframe
          \sectionpage
        \else
          \frame[c]{\sectionpage}
        \fi
      }
    }
    \metropolis@enablesectionpage
    \makeatother
  #define footer of section pages:
  - |
    \makeatletter
    \def\ps@sectionpage{%
      \setbeamertemplate{frame footer}{\tiny{\textcolor{beaublue}{Workshop - Data Wrangling of Web Data in R | SIMON RESS}}}
    }
    \addtobeamertemplate{section page}{\thispagestyle{sectionpage}}{}
    \makeatother
#add section numbers to TOC:
  - |
    \setbeamertemplate{section in toc}{
    \leavevmode%
    \inserttocsectionnumber. 
    \inserttocsection\par%
    }
    \setbeamertemplate{subsection in toc}{
    \leavevmode\leftskip=2.5em\inserttocsubsection\par}
#Adjust representation of chunks
  #Reduce space between code chunks and code output
  - |
    \setlength{\OuterFrameSep}{-4pt}
    \makeatletter
    \preto{\@verbatim}{\topsep=-10pt \partopsep=-10pt }
    \makeatother
  #Change background-color of source-code
  - \definecolor{shadecolor}{RGB}{240,240,240}
  #Set a frame around the results
  - | 
    \let\verbatim\undefined
    \let\verbatimend\undefined
    \DefineVerbatimEnvironment{verbatim}{Verbatim}{frame=single, rulecolor=\color{shadecolor}, framerule=0.3mm,framesep=1mm}
#enable line breaks in chunks
  - |
    \usepackage{fvextra}
    \DefineVerbatimEnvironment{Highlighting}{Verbatim}{breaklines,commandchars=\\\{\}}
#   \DefineVerbatimEnvironment{verbatim}{Verbatim}{breaklines,commandchars=\\\{\}}
---



### Content
\tableofcontents[]

# Setup

### Target

**Meta information**

- Finanzausschuss
- Ausschüsse der 19. Wahlperiode (2017-2021)
- Öffentliche Anhörungen

URL: https://www.bundestag.de/webarchiv/Ausschuesse/ausschuesse19/a07/Anhoerungen

**Unit information** 

- Committees

URL: Needs to be scraped from main page


### Configurate & Start Selenium/Browser




```r
library(RSelenium)
library(rvest) #for read_html(), html_elements()...
#Free all ports
  system("taskkill /im java.exe /f", intern=FALSE, ignore.stdout=FALSE)
#Start a selenium & Assign client to an R-object  
  rD <- rsDriver(port = 4561L, browser = "firefox")
  remDr <- rD[["client"]]
  #remDr$quit
```



# Functions

### Overview

- Functions are **blocks of codes** which can be executed repeatedly by calling them
- **Parameters** (data) can be passed into them, which are used by the code inside
- **Data can be returned** from a function

*Syntax:*

```r
function_name <- function(arg_1, arg_2, ...) {
     Function body 
}
```

### Function Components
The different parts of a function are:

- **Function Name:** This is the actual name of the function. It is stored in R environment as an object with this name.
- **Arguments:** An argument is a placeholder. When a function is invoked, you pass a value to the argument. Arguments are *optional*; that is, a function may contain no arguments. Also arguments *can* have default values.
- **Function Body:** The function body contains a collection of statements that defines what the function does.
- **Return Value:** The return value of a function is the last expression in the function body to be evaluated.

### Examplary Function 


```r
square <- function(value = 1, factor = 1) {
     return(value^factor)
}
square() #use defaut args
```

```
## [1] 1
```

```r
square(2,3) #use args by position
```

```
## [1] 8
```

```r
square(factor=2, value=5) #use args by name
```

```
## [1] 25
```



## Define savepage()


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

## Usage of savepage()

```r
#navigate to url & save content as r-object
page <- savepage("https://www.bundestag.de/ webarchiv/Ausschuesse/ausschuesse19/a07/ Anhoerungen")
page
```


```
## {html_document}
## <html xml:lang="de" dir="ltr" class="detection-firefox" lang="de">
## [1] <head>\n<meta http-equiv="Content-Type" content="text/html; charset=UTF-8 ...
## [2] <body class="bt-archived-page">\n  <div class="bt-archive-banner">\n    < ...
```

# Loops & apply-family

## Overview

## for-loop


## while-loop
- With the while loop we can execute a set of statements as long as a condition is TRUE
- With the break statement, we can stop the loop even if the while condition is TRUE:
- With the next statement, we can skip an iteration without terminating the loop:


```r
i <- 1
while (i < 6) {
  print(i)
  i <- i + 1
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

## apply functions

# Dplyr - Gramma of Data Manipulation

## Overview


# Purr

## Overview
"purrr enhances R’s functional programming (FP) toolkit by providing a complete and consistent set of tools for working with functions and vectors."


```r
if(!require("purrr")) install.packages("purrr") 
  library(purrr) # for fill() 
mtcars %>%
  split(.$cyl) %>% # from base R
  map(~ lm(mpg ~ wt, data = .)) %>%
  map(summary) %>%
  map_dbl("r.squared")
```

```
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

# Helpful Sources
- [purr: Overview](https://purrr.tidyverse.org/)
- [purr: References](https://purrr.tidyverse.org/reference/index.html)
- [purr: Cheatsheet](https://github.com/rstudio/cheatsheets/blob/master/purrr.pdf)





# Helpful sources

- [Stringr: Overview](https://stringr.tidyverse.org/)
- [Stringr: Introduction](https://cloud.r-project.org/web/packages/stringr/vignettes/stringr.html)
- [Stringr: Cheatsheet](https://github.com/rstudio/cheatsheets/blob/master/strings.pdf)
- [Stringr: Reference manual](https://cloud.r-project.org/web/packages/stringr/stringr.pdf)
- [Base R String-functions vs Stringr](https://stringr.tidyverse.org/articles/from-base.html)
- [Working with strings in R](https://r4ds.had.co.nz/strings.html)
- [Regular expressions](https://cloud.r-project.org/web/packages/stringr/vignettes/regular-expressions.html)
- [Primary R functions for dealing with regular expressions](https://bookdown.org/rdpeng/rprogdatascience/regular-expressions.html)


### References
All graphics are taken from [String manipulaton with stringr Cheatsheet](https://github.com/rstudio/cheatsheets/blob/master/strings.pdf)
