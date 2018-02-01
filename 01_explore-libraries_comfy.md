01\_explore-libraries\_comfy.R
================
clecroy
Wed Jan 31 15:53:44 2018

``` r
library(tidyverse)
```
Remotely added line to conflict with local.

    ## -- Attaching packages --------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 2.2.1     v purrr   0.2.4
    ## v tibble  1.4.2     v dplyr   0.7.4
    ## v tidyr   0.7.2     v stringr 1.2.0
    ## v readr   1.1.1     v forcats 0.2.0

    ## -- Conflicts ------------------------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

Which libraries does R search for packages?

``` r
# try .libPaths(), .Library
```

Installed packages

``` r
## use installed.packages() to get all installed packages
## if you like working with data frame or tibble, make it so right away!
## remember to use View() or similar to inspect

## how many packages?
pkgs <- installed.packages()
pkgs <- as_tibble(pkgs)
pkgs %>% summarize(n())
```

    ## # A tibble: 1 x 1
    ##   `n()`
    ##   <int>
    ## 1   123

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
##   * what proportion need compilation?
##   * how break down re: version of R they were built on
pkgs %>% group_by(LibPath, Priority) %>% summarize(n())
```

    ## # A tibble: 4 x 3
    ## # Groups:   LibPath [?]
    ##   LibPath                                      Priority    `n()`
    ##   <chr>                                        <chr>       <int>
    ## 1 C:/Program Files/R/R-3.4.3/library           base           14
    ## 2 C:/Program Files/R/R-3.4.3/library           recommended    15
    ## 3 C:/Program Files/R/R-3.4.3/library           <NA>            1
    ## 4 C:/Users/clecroy/Documents/R/win-library/3.4 <NA>           93

``` r
pkgs %>% count(NeedsCompilation)
```

    ## # A tibble: 3 x 2
    ##   NeedsCompilation     n
    ##   <chr>            <int>
    ## 1 no                  53
    ## 2 yes                 64
    ## 3 <NA>                 6

``` r
pkgs %>% count(LibPath, Priority)
```

    ## # A tibble: 4 x 3
    ##   LibPath                                      Priority        n
    ##   <chr>                                        <chr>       <int>
    ## 1 C:/Program Files/R/R-3.4.3/library           base           14
    ## 2 C:/Program Files/R/R-3.4.3/library           recommended    15
    ## 3 C:/Program Files/R/R-3.4.3/library           <NA>            1
    ## 4 C:/Users/clecroy/Documents/R/win-library/3.4 <NA>           93

``` r
pkgs %>% count(NeedsCompilation) %>% mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                  53 0.431 
    ## 2 yes                 64 0.520 
    ## 3 <NA>                 6 0.0488

``` r
pkgs %>% count(Built) %>% mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   Built     n    prop
    ##   <chr> <int>   <dbl>
    ## 1 3.4.1    10 0.0813 
    ## 2 3.4.2     1 0.00813
    ## 3 3.4.3   112 0.911

``` r
## for tidyverts, here are some useful patterns
# data %>% count(var)
# data %>% count(var1, var2)
# data %>% count(var) %>% mutate(prop = n / sum(n))
```

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
## study package naming style (all lower case, contains '.', etc
## use `fields` argument to installed.packages() to get more info and use it!

installed.packages(fields = "URL") %>% as_tibble %>% select(URL) %>% grep(pattern = "github")
```

    ## [1] 1

``` r
sum(grepl("[A-Z]", pkgs$Package))
```

    ## [1] 14

``` r
mean(grepl("[A-Z]", pkgs$Package))
```

    ## [1] 0.1138211

``` r
sessionInfo()
```

    ## R version 3.4.3 (2017-11-30)
    ## Platform: x86_64-w64-mingw32/x64 (64-bit)
    ## Running under: Windows 7 x64 (build 7601) Service Pack 1
    ## 
    ## Matrix products: default
    ## 
    ## locale:
    ## [1] LC_COLLATE=English_United States.1252 
    ## [2] LC_CTYPE=English_United States.1252   
    ## [3] LC_MONETARY=English_United States.1252
    ## [4] LC_NUMERIC=C                          
    ## [5] LC_TIME=English_United States.1252    
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ##  [1] bindrcpp_0.2    forcats_0.2.0   stringr_1.2.0   dplyr_0.7.4    
    ##  [5] purrr_0.2.4     readr_1.1.1     tidyr_0.7.2     tibble_1.4.2   
    ##  [9] ggplot2_2.2.1   tidyverse_1.2.1
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_0.12.15     cellranger_1.1.0 pillar_1.1.0     compiler_3.4.3  
    ##  [5] plyr_1.8.4       bindr_0.1        tools_3.4.3      digest_0.6.14   
    ##  [9] lubridate_1.7.1  jsonlite_1.5     evaluate_0.10.1  nlme_3.1-131    
    ## [13] gtable_0.2.0     lattice_0.20-35  pkgconfig_2.0.1  rlang_0.1.6     
    ## [17] psych_1.7.8      cli_1.0.0        rstudioapi_0.7   yaml_2.1.16     
    ## [21] parallel_3.4.3   haven_1.1.1      xml2_1.2.0       httr_1.3.1      
    ## [25] knitr_1.19       hms_0.4.1        rprojroot_1.3-2  grid_3.4.3      
    ## [29] glue_1.2.0       R6_2.2.2         readxl_1.0.0     foreign_0.8-69  
    ## [33] rmarkdown_1.8    modelr_0.1.1     reshape2_1.4.3   magrittr_1.5    
    ## [37] backports_1.1.2  scales_0.5.0     htmltools_0.3.6  rvest_0.3.2     
    ## [41] assertthat_0.2.0 mnormt_1.5-5     colorspace_1.3-2 utf8_1.1.3      
    ## [45] stringi_1.1.6    lazyeval_0.2.1   munsell_0.4.3    broom_0.4.3     
    ## [49] crayon_1.3.4
