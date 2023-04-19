Exercise 2 - Data Curation
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

1.  Install the `weights` package by running
    `install.packages('weights')`

2.  Install the `tidyfst` package by running
    `install.packages('tidyfst')`

3.  Install the `rmarkdown` package by running
    `install.packages('rmarkdown')`

## How to Import Data in R

There are [a lot of packages in the `tidyverse` for importing
data](https://www.tidyverse.org/packages/#import), but you should mostly
to only care about [`readr`](https://readr.tidyverse.org/)† and
[`readxl`](https://readxl.tidyverse.org/)‡.

- `read_csv()`† for comma-separated values (csv) files
- `read_csv2()`† for csv files that use semicolons as delimiters
- `read_tsv()`† for csv files that use tabs as delimiters
- `read_delim()`† for csv files that use anything else as delimiters
- `read_excel()`‡ for Excel files

As you can see, there are a lot of different functions with very
specific use cases.

**Important**: There are also very similar functions from other (partly
the basic) packages, which are called for example `read.csv()`. Pay
attention to the correct notation/source package accordingly. That
means: Remember to load (and install if necessary) the necessary package
to be able to use the functions.

To read a file, it’s easiest if the file is in your current working
directory (CWD). To find out what your CWD is, use `getwd()`.

``` r
getwd()
```

### Necessary CSV File

Download the CSV file [friends.csv](./friends.csv) from GitHub to your
private computer. To get the file, you can either download everything as
`.zip` or create a new empty file and copy&paste the raw file content
(for those of you who know what that is, you can also, of course, clone
the repository).

- **Option 1**: Download as `.zip`: Go to [the `wellbeing`
  repository](https://github.com/tud-ise/Wellbeing_SoSe2023), click on
  the green `Code` button and click on “Download ZIP”. Then put it into
  your CWD. Also see: [“How to download as
  ZIP?”](https://stackoverflow.com/questions/2751227/how-to-download-source-in-zip-format-from-github)

- **Option 2**: Create an empty file and copy&paste: Open RStudio, type
  `write.csv('hello there', 'friends.csv')` and press `Enter`. You
  should now see a new file in RStudio (Section “files”) that you can
  open (Select the file and press “View File”) and replace the content
  with [the raw content of the file in the
  repository](https://raw.githubusercontent.com/tud-ise/Wellbeing_SoSe2023/main/Exercises/Exercise%202/friends.csv).
  Don’t forget to save the changes.

- **Option 3**: Use `getwd()` to read your current working directory and
  copy it to your clipboard. Then open [the raw content of the file in
  the
  repository](https://raw.githubusercontent.com/tud-ise/Wellbeing_SoSe2023/main/Exercises/Exercise%202/friends.csv),
  use the `right mouse button` and click “`Save as`”. Save the csv file
  (pay attention to the file type) in your working directory.

After the file itself is created, we should also save it to a variable.

In the so created tibble, we can see that three observations in the
“smartphone” column have no entry (`NA`). For our first evaluation, we
are **not interested in which smartphone** a character uses, but **only
whether s/he has one or not**. For this we can use dummy variables.

## Dummy Variables

To make our lives easier, it is possible in R to define our own
functions and then use them. This is very simple:

``` r
ourFunction <- function() {

}
```

Let’s go through these lines step by step. We see that we define an
object name, `ourFunction`, and this object is now assigned something,
`<-`. With the keyword `function` we say that `ourFunction` should be a
function. In our example, the function has no parameters, so there is
nothing in the braces, `()`. The curly braces form the `function body`,
which now contains all the statements that belong to the function.
Everything after the closing brace no longer belongs to the function.†

† [Source](https://r-coding.de/blog/funktionen-in-r/)

But now we want the function to do something specific, because it is
still empty and nothing is executed. Let’s build a function that
calculates a simple mathematical function:

``` r
ourFunction <- function(x) {
  result <- 9*x + 42
  return(result)
}

ourFunction(3)
```

    ## [1] 69

For our use case, we first need our own defined function
`has_a_smartphone()` that returns `1` if the passed parameter is defined
and `0` if not. We can declare our own function as follows:

``` r
has_a_smartphone <- function(smartphone) {
  has_smartphone = ifelse(is.na(smartphone), 0, 1)
  return(has_smartphone)
}
```

Now, we want to use this function to dummify the column `smartphone`
into a new column `has_smartphone`. `1` means “s/he has a smartphone”
and `0` means “s/he has no smartphone”. It **doesn’t matter which**
smartphone the character has, **only** **if** they have one.

``` r
friends %>% 
  mutate(has_smartphone = has_a_smartphone(smartphone))
```

    ## # A tibble: 14 × 3
    ##    name      smartphone has_smartphone
    ##    <chr>     <chr>               <dbl>
    ##  1 Luke      JediCom                 1
    ##  2 Leia      HoloPhone               1
    ##  3 Han       SaberPhone              1
    ##  4 Rey       JediCom                 1
    ##  5 Chewbacca <NA>                    0
    ##  6 C-3PO     DroidLink               1
    ##  7 R2-D2     DroidLink               1
    ##  8 Obi-Wan   JediCom                 1
    ##  9 JarJar    <NA>                    0
    ## 10 Boba      <NA>                    0
    ## 11 Lando     SaberPhone              1
    ## 12 Yoda      <NA>                    0
    ## 13 Qui-Gon   JediCom                 1
    ## 14 Jabba     <NA>                    0

**Now, we do want to know which smartphone** each character uses In
theory, we could do this by checking for each possibility separately:

But, as you can see, this is quite tedious. Luckily, there is the
function `dummify()` from the package `weights`. So load the package
with `library(weights)` (install if necessary with
`install.packages('weights')`and then let’s try to use `dummify()`:

``` r
library(weights)
dummify(friends$smartphone)
```

This will show you an error: `variable needs to be a factor`. But what
are factors?

> R uses factors to represent categorical variables that have a known
> set of possible values.†

† [Source](https://r4ds.had.co.nz/factors.html?q=fac#creating-factors)

So how do we create factors? In this case, it’s simple. First, we remove
all `NA` values:

``` r
friends <- friends %>% 
  mutate(smartphone = replace_na(smartphone, 'NONE'))
```

Then, we make the `vaccine` column a factor:

``` r
friends_factorized <- friends %>% 
  mutate(smartphone = as.factor(smartphone))
```
