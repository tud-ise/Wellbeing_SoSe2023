Exercise 3 - Working with Dates
================

## Christopher Diebel & Jan-Hendrik Schmidt - ISE Darmstadt

**Note**: Thanks are also due here to Gregor Albrecht and Jakob Lorz,
whose exercise session from the summer semester of 2021 also forms the
basis of this semester’s exercise.

- :house_with_garden: [Home](./README.md)

- :open_book: [`dplyr`
  Documentation](https://dplyr.tidyverse.org/reference/index.html)

- :open_book: [`readr`
  Documentation](https://readr.tidyverse.org/reference/index.html)

- :open_book: [`lubridate`
  Documentation](https://lubridate.tidyverse.org/reference/index.html)

- :open_book: [`rmarkdown` Cheatsheet](./rmarkdown-cheatsheet-2.0.pdf)

- :open_book: Further Reading [R for Data
  Science](https://r4ds.had.co.nz/), [R für
  Einsteiger](http://aproxy.ulb.tu-darmstadt.de:2058/book/index.cfm?bok_id=1993358)
  or [Einführung in
  R](https://methodenlehre.github.io/einfuehrung-in-R/index.html)

## Preparation

1.  Install the `lubridate` package by running
    `install.packages('lubridate')`

## Working with Dates

Because working with dates can be cumbersome, the `tidyverse` contains a
very helpful package for that: `lubridate`.

``` r
library(lubridate)
```

    ## Warning: package 'lubridate' was built under R version 4.2.3

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

If the date you are trying to work with contains a date like `4/14/22`,
you can tell `lubridate` that this is a date:

``` r
date <- parse_date_time('4/14/22', 'mdy')
```

The `'mdy'` represents `[m]onth [d]ay [year]`. The resulting `date` is a

“POSIXct date-time object” (you can verify that with `is.instant(date)`
or `class(date)`) that makes it very easy to work with it. For instance,
you can find out which week the date refers to:

``` r
week(date)
```

    ## [1] 15

We can also do some calculations with timestamps:

``` r
start <- now()

# wait...
Sys.sleep(3.14159265359)
end <- now()

elapsed <- start %--% end
# ... and find out how long you waited
as.duration(elapsed)
```

    ## [1] "3.22392702102661s"
