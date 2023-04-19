Exercise 2 - Functions Exercises
================

## May the Functions Be With You

#### Mastering Function Definition and Manipulation in R with the Star Wars Universe

Complete the following tasks using the `starwars` dataset from the
tidyverse package:

Example: Define a function that takes a character’s name as an argument
and returns their species.

``` r
get_species <- function(name) {
  species <- starwars %>% filter(name == !!name) %>% select(species) %>% pull()
  return(species)
}

get_species("Luke Skywalker")
```

The `!!` before the name argument is a syntax called “bang bang”
operator, which tells R to evaluate the argument immediately instead of
delaying it. This is because the argument name is used within the filter
function, which requires immediate evaluation of the argument.

1\. Define a function that takes in a character’s name and returns a
dataframe containing the name, birth year, and species of that
character.

2\. Define a function that takes in a character’s name and returns their
height in meters.

3\. Define a function that takes in a planet’s name and returns the
number of characters whose homeworld is this planet.

4\. Define a function that takes in a planet name and returns a
dataframe containing the names and heights of all characters from that
planet, sorted by height in descending order.

5\. Define a function that takes a character’s name as an argument and
returns the average height of all characters from the same species as
that character.

6\. Define a function that takes in a character’s name and a planet
name, and returns a dataframe containing the name and gender of all
characters from that planet who share the same gender as the input
character.
