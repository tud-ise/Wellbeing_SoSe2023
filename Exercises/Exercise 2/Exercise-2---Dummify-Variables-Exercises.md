Exercise 2 - Dummify Variables Exercises
================

## Dummify Variables

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

5.  How many characters starred in `A New Hope`?

6.  Which of those characters are female?
