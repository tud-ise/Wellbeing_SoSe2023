Exercise 2 - Introductory Exercises
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

3\. How many characters are there per species? Sort by descending count.

``` r
starwars %>%
    group_by(species) %>%
    summarise(count = n()) %>%
    arrange(desc(count))
```

4\. Change the height from `cm` to `m`.

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
# A tibble: 11 x 2
   name              eye_color
   <chr>             <chr>
 1 C-3PO             yellow
 2 Darth Vader       yellow
 3 Palpatine         yellow
 4 Watto             yellow
 5 Darth Maul        yellow
 6 Dud Bolt          yellow
 7 Ki-Adi-Mundi      yellow
 8 Yarael Poof       yellow
 9 Poggle the Lesser yellow
10 Zam Wesell        yellow
11 Dexter Jettster   yellow
```

2\. Remove all Gungans. How many characters are left?

``` r
> starwars %>%
+     filter(is.na(species)|species != 'Gungan')
# A tibble: 84 x 14
```

3\. What is the average mass of all droids?

``` r
> starwars %>%
+     select(name, species, mass) %>%
+     filter(species == 'Droid') %>%
+     na.omit() %>%
+     summarise(mean(mass))
# A tibble: 1 x 1
  `mean(mass)`
         <dbl>
1         69.8
```

4\. Calculate the BMI for all humans (\`mass / ((height / 100) ^ 2)\`)

``` r
> starwars %>%
+     select(name, mass, height, species) %>%
+     filter(species == 'Human') %>%
+     filter(!is.na(mass)) %>%
+     filter(!is.na(height)) %>%
+     mutate(bmi = (mass / ((height / 100) ^ 2)))
# A tibble: 22 x 5
   name                mass height species   bmi
   <chr>              <dbl>  <int> <chr>   <dbl>
 1 Luke Skywalker        77    172 Human    26.0
 2 Darth Vader          136    202 Human    33.3
 3 Leia Organa           49    150 Human    21.8
 4 Owen Lars            120    178 Human    37.9
 5 Beru Whitesun lars    75    165 Human    27.5
 6 Biggs Darklighter     84    183 Human    25.1
 7 Obi-Wan Kenobi        77    182 Human    23.2
 8 Anakin Skywalker      84    188 Human    23.8
 9 Han Solo              80    180 Human    24.7
10 Wedge Antilles        77    170 Human    26.6
# … with 12 more rows
```

5\. Which character has the longest name?

``` r
> starwars %>%
+     select(name) %>%
+     arrange(desc(str_length(name))) %>%
+     filter(row_number()==1)
# A tibble: 1 x 1
   name
   <chr>
 1 Jabba Desilijic Tiure
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
# A tibble: 15 x 2
   species        `max(birth_year)`
   <chr>                      <dbl>
 1 Cerean                        92
 2 Droid                        112
 3 Ewok                           8
 4 Gungan                        52
 5 Human                        102
 6 Hutt                         600
 7 Kel Dor                       22
 8 Mirialan                      58
 9 Mon Calamari                  41
10 Rodian                        44
11 Trandoshan                    53
12 Twi'lek                       48
13 Wookiee                      200
14 Yoda's species               896
15 Zabrak                        54
```

7\. What are the most populated planets?

``` r
> starwars %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
# A tibble: 49 x 2
   homeworld     n
   <chr>     <int>
 1 Naboo        11
 2 Tatooine     10
 3 NA           10
 4 Alderaan      3
 5 Coruscant     3
 6 Kamino        3
 7 Corellia      2
 8 Kashyyyk      2
 9 Mirial        2
10 Ryloth        2
# … with 39 more rows
```

8\. What is the average height of all female humans?

``` r
> starwars %>%
+     select(name, height, gender, species) %>%
+     filter(species == 'Human', gender == 'feminine') %>%
+     na.omit() %>%
+     summarise(mean(height))
# A tibble: 1 x 1
  `mean(height)`
           <dbl>
1           160.
```

9\. Which planet has the most droids?

``` r
> starwars %>%
+     filter(species == 'Droid') %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
# A tibble: 3 x 2
  homeworld     n
  <chr>     <int>
1 NA            3
2 Tatooine      2
3 Naboo         1
```

10\. Which is the most prevalent eye color on each planet?

``` r
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species, name) %>%
+     summarise(by = max(birth_year)) %>%
+     group_by(species) %>%
+     slice(which.max(by))
`summarise()` has grouped output by 'species'. You can override using the `.groups` argument.
# A tibble: 15 x 3
# Groups:   species [15]
   species        name                     by
   <chr>          <chr>                 <dbl>
 1 Cerean         Ki-Adi-Mundi             92
 2 Droid          C-3PO                   112
 3 Ewok           Wicket Systri Warrick     8
 4 Gungan         Jar Jar Binks            52
 5 Human          Dooku                   102
 6 Hutt           Jabba Desilijic Tiure   600
 7 Kel Dor        Plo Koon                 22
 8 Mirialan       Luminara Unduli          58
 9 Mon Calamari   Ackbar                   41
10 Rodian         Greedo                   44
11 Trandoshan     Bossk                    53
12 Twi'lek        Ayla Secura              48
13 Wookiee        Chewbacca               200
14 Yoda's species Yoda                    896
15 Zabrak         Darth Maul               54
```

11\. How many unique eye colors are there?

``` r
> starwars %>%
+       summarise(eye_color) %>%
+       distinct(eye_color)
# A tibble: 15 x 1
   eye_color
   <chr>
 1 blue
 2 yellow
 3 red
 4 brown
 5 blue-gray
 6 black
 7 orange
 8 hazel
 9 pink
10 unknown
11 red, blue
12 gold
13 green, yellow
14 white
15 dark
```
