Exercise 2 - Practice
================

# Welcome to the Star Wars Universe!

## Recap: Last Week’s `Star Wars`Homework

1\. How many droids are there?

``` r
starwars %>%
   filter(species == 'Droid') %>%
   summarise(n())
```

2\. What is the home world of Han Solo?

``` r
starwars %>%
   select(name, homeworld) %>%
   filter(name == 'Han Solo')
```

3\. How many characters are there per species? Sort by ascending count.

``` r
starwars %>%
    group_by(species) %>%
    summarise(count = n()) %>%
    arrange(desc(count))
```

4\. Change the height from ´cm´ to ´m´.

``` r
starwars %>%
   mutate(height = height / 100)
```

5\. Filter out all characters that have missing values (´NA\`) for their
mass. How many are left?

``` r
starwars %>%
   select(name, mass) %>%
   na.omit()
```

## Further `Star Wars` Tasks - Breakout Rooms

1\. Find all characters with yellow eyes.

2\. Remove all Gungans. How many characters are left?

3\. What is the average mass of all droids?

4\. Calculate the BMI for all humans (\`mass / ((height / 100) ^ 2)\`)

5\. Which character has the longest name?

6\. What is the earliest birth year for each species (\`birth_year\` is
measured in BBY = Before Battle of Yavin, so high values = earlier)

7\. What are the most populated planets?

8\. What is the average height of all female humans?

9\. Which planet has the most droids?

10\. Which is the most prevalent eye color on each planet?

11\. How many unique eye colors are there?
