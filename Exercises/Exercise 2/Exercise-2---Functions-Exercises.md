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

``` r
character_info <- function(name) {
  starwars %>% 
    filter(name == !!name) %>% 
    select(name, birth_year, species)
}

character_info("Darth Vader")
```

2\. Define a function that takes in a character’s name and returns their
height in meters.

``` r
height_in_meters <- function(name) {
  starwars %>% 
    filter(name == !!name) %>% 
    pull(height) %>% 
    as.numeric() / 100
}
height_in_meters("Luke Skywalker")
```

3\. Define a function that takes in a planet’s name and returns the
number of characters whose homeworld is this planet.

``` r
get_num_characters_from_planet <- function(planet_name) {
  num_characters <- starwars %>% 
    filter(homeworld == !!planet_name) %>% 
    nrow()
  return(num_characters)
}
get_num_characters_from_planet("Tatooine")
```

4\. Define a function that takes in a planet name and returns a
dataframe containing the names and heights of all characters from that
planet, sorted by height in descending order.

``` r
characters_from_planet <- function(planet_name) {
  starwars %>% 
    filter(homeworld == !!planet_name) %>% 
    select(name, height) %>% 
    arrange(desc(height))
}

characters_from_planet("Tatooine")
```

5\. Define a function that takes a character’s name as an argument and
returns the average height of all characters from the same species as
that character.

``` r
get_avg_height_same_species <- function(name) {
  species <- starwars %>% 
    filter(name == !!name) %>% 
    select(species) %>% 
    pull()
  avg_height <- starwars %>% 
    filter(species == !!species) %>% 
    summarise(avg_height = mean(height, na.rm = TRUE)) %>%
    pull()
  return(avg_height)
}
get_avg_height_same_species("Luke Skywalker")
```

6\. Define a function that takes in a character’s name and a planet
name, and returns a dataframe containing the name and gender of all
characters from that planet who share the same gender as the input
character.

``` r
characters_same_gender <- function(char_name, planet_name) {
  starwars %>% 
    filter(homeworld == !!planet_name, gender == starwars %>% 
             filter(name == !!char_name) %>% 
             pull(gender)) %>% 
    select(name, gender)
}
characters_same_gender("Luke Skywalker", "Tatooine")
```
