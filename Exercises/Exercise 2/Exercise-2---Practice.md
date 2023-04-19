Exercise 2 - Practice
================

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

``` r
> starwars %>%
+     select(name, eye_color) %>%
+     filter(eye_color == 'yellow')
```

2\. Remove all Gungans. How many characters are left?

``` r
> starwars %>%
+     filter(is.na(species)|species != 'Gungan')
```

3\. What is the average mass of all droids?

``` r
> starwars %>%
+     select(name, species, mass) %>%
+     filter(species == 'Droid') %>%
+     na.omit() %>%
+     summarise(mean(mass))
```

4\. Calculate the BMI for all humans (\`mass / ((height / 100) ^ 2)\`)

``` r
> starwars %>%
+     select(name, mass, height, species) %>%
+     filter(species == 'Human') %>%
+     filter(!is.na(mass)) %>%
+     filter(!is.na(height)) %>%
+     mutate(bmi = (mass / ((height / 100) ^ 2)))
```

5\. Which character has the longest name?

``` r
> starwars %>%
+     select(name) %>%
+     arrange(desc(str_length(name))) %>%
+     filter(row_number()==1)
```

6\. What is the earliest birth year for each species (\`birth_year\` is
measured in BBY = Before Battle of Yavin, so high values = earlier)

``` r
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species) %>%
+     arrange(birth_year) %>%
+     summarise(max(birth_year))
```

7\. What are the most populated planets?

``` r
> starwars %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
```

8\. What is the average height of all female humans?

``` r
> starwars %>%
+     select(name, height, gender, species) %>%
+     filter(species == 'Human', gender == 'feminine') %>%
+     na.omit() %>%
+     summarise(mean(height))
```

9\. Which planet has the most droids?

``` r
> starwars %>%
+     filter(species == 'Droid') %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
```

10\. Who is the oldest character of each species?

``` r
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species, name) %>%
+     summarise(by = max(birth_year)) %>%
+     group_by(species) %>%
+     slice(which.max(by))
```

11\. Which is the most prevalent eye color on each planet?

``` r
> starwars %>%
+       group_by(homeworld, eye_color) %>%
+       mutate(rank = row_number()) %>%
+       group_by(homeworld) %>%
+       slice(which.max(rank)) %>%
+       summarise(homeworld, eye_color, rank)
```

12\. How many unique eye colors are there?

``` r
> starwars %>%
+       summarise(eye_color) %>%
+       distinct(eye_color)
```
