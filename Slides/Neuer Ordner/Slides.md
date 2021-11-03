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







# Sub-Problems

```r
#Sub-problem 1
page <- savepage("https://www.bundestag.de/webarchiv/Ausschuesse/ausschuesse19/a07/Anhoerungen")
#Sub-problem 2
system("taskkill /im java.exe /f", intern=FALSE, ignore.stdout=FALSE)
```

```r
#Sub-problem 3
html_element(page, xpath="//meta[10]") %>%
  html_attr("content")
```

```
## [1] "https://www.bundestag.dehttps://www.bundestag.de/resource/image/244626/2x3/316/475/b16fd9b7ae2fc1b1e097c016394bdcb6/ie/default.jpg"
```

```r
#Sub-problem 4
html_element(page,xpath="//meta[3]")
```

```
## {html_node}
## <meta name="viewport" content="width=device-width, initial-scale=1">
```
