Exercise 3 - Data Curation and Dates
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

## Practice: Breakout Session

1.  Install the `tidyfst` package: `install.packages("tidyfst")`.
    Afterwards, load it with `library(tidyfst)`.

2.  Download the `starwars_dd.csv` file from GitHub. For ease of use:
    place it in the folder which is shown in the `Files` section of
    RStudio.

The following code was used to generate the `starwars_dd.csv` file, in
case you are curious (this is not part of this exercise).

``` r
> library(tidyverse)
> starwars %>% 
+       unnest(cols=films,keep_empty=TRUE) %>% 
+       unnest(cols=vehicles, keep_empty=TRUE) %>% 
+       unnest(cols=starships, keep_empty=TRUE) %>%
+       write.csv('./starwars_dd.csv')
```

3.  Import `starwars_dd.csv` into a variable named `starwars_dd`. The
    contents are different from the tidyverse-internal `starwars`
    dataset and simplify the next steps for you.

``` r
> read_csv('./starwars_dd.csv') -> starwars_dd
```

4.  Import the `tidyfst` package. Then generate dummies for all films
    and store them in the variable `starwars_films_dd`. Inspect them in
    RStudio (using the `View` function) afterwards. You may use the
    [:book:
    `dummy_dt`](https://www.rdocumentation.org/packages/tidyfst/versions/0.9.9/topics/dummy_dt)
    function from `tidyfst`. Make sure to exclude rows which hold no
    additional information, we are focusing on the different films here
    (Hint: Each character has multiple rows for each film, vehicle and
    starship)

``` r
> library(tidyfst)
> starwars_dd %>% 
+       distinct(name, films, .keep_all=TRUE) %>% 
+       dummy_dt(films) ->
+       starwars_films_dd
> starwars_films_dd <- as_tibble(starwars_films_dd)
> View(starwars_films_dd)
```

5.  How many characters starred in `A New Hope`?

``` r
> starwars_films_dd %>% 
+       filter(`films_A New Hope` == 1) %>% 
+       summarise(name, `films_A New Hope`)
# A tibble: 18 x 2
name                  `films_A New Hope`
<chr>                              <dbl>
1 Beru Whitesun lars                     1
2 Biggs Darklighter                      1
3 C-3PO                                  1
4 Chewbacca                              1
5 Darth Vader                            1
6 Greedo                                 1
7 Han Solo                               1
8 Jabba Desilijic Tiure                  1
9 Jek Tono Porkins                       1
10 Leia Organa                            1
11 Luke Skywalker                         1
12 Obi-Wan Kenobi                         1
13 Owen Lars                              1
14 R2-D2                                  1
15 R5-D4                                  1
16 Raymus Antilles                        1
17 Wedge Antilles                         1
18 Wilhuff Tarkin                         1 
```

6.  Which of those characters are female?

``` r
> starwars_films_dd %>% 
+       filter(`films_A New Hope` == 1) %>% 
+       filter(sex == 'female') %>% 
+       summarise(name, `films_A New Hope`)
# A tibble: 2 x 2
  name               `films_A New Hope`
  <chr>                           <dbl>
1 Beru Whitesun lars                  1
2 Leia Organa                         1
```

## Group Session: Working with Dates

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

    ## [1] "3.22287511825562s"
