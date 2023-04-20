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
> starwars_dd <- read_csv('./starwars_dd.csv')
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
+       dummy_dt(films) -> starwars_films_dd

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

------------------------------------------------------------------------

:heavy_exclamation_mark: **In the exercise, this call did not work
because we confused single quotes (`'`) with backticks (`` ` ``)**.

In R, the backtick character is used to enclose column names that
include spaces, reserved words, or other special characters. This is
because column names with spaces or special characters may cause syntax
errors when they are not enclosed properly.

For example, suppose you have a data frame with a column named “First
Name”. If you try to reference this column using single quotes or double
quotes like `df['First Name']` or `df["First Name"]`, you may get an
error message like `"Error: object 'First' not found"` because R
interprets the space as a separation between two different objects.

To avoid this error, you can enclose the column name in backticks like
\`df\$\`First Name\`\` or \`df\[\[“First Name”\]\]\`. The backticks tell
R to treat everything inside them as a single entity, so the column name
with spaces is interpreted correctly.

Alternatively, you can also rename the column to remove the spaces or
special characters using the `colnames()` function, for example:
`colnames(df)[2] <- "FirstName"`.

------------------------------------------------------------------------

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
