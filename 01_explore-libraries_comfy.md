01\_explore-libraries\_comfy.R
================
clecroy
Wed Jan 31 14:08:54 2018

Which libraries does R search for packages?

``` r
# try .libPaths(), .Library
```

Installed packages

``` r
## use installed.packages() to get all installed packages
## if you like working with data frame or tibble, make it so right away!
## remember to use View() or similar to inspect
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 2.2.1     v purrr   0.2.4
    ## v tibble  1.4.2     v dplyr   0.7.4
    ## v tidyr   0.7.2     v stringr 1.2.0
    ## v readr   1.1.1     v forcats 0.2.0

    ## -- Conflicts ------------------------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
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
