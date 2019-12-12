---
title: "Data Analysis with dplyr"
author: "Subha Yoganandan"
date: "12/12/2019"
output:
  html_document:
    keep_md: yes
  pdf_document: default
  word_document: default
---


```r
knitr::opts_chunk$set(echo = TRUE)
```

## Data Analysis with dplyr

The advent of several "point and click" or "drag and drop" tools have eased data manipulation for analysts. Data retrieval, wrangling and cleansing is becoming a task which anyone can perform. However in some scenarios such tools fail to manipulate advanced or complex analysis without the inclusion of typing in programming lines into custom transfiguration. Tools like R and Python are still the most preferred tools due to their advanced nature of handling statistical analysis and machine learning complexity.

Every Data Scientist or Analyst spends about 80% of his/her time in data wrangling and only about 20% of the time in the actual analysis. Many packages in R programming language are quite extensively used for complex data retrievals. The dplyr package is pretty neat in performing several data munging tasks. In this blog I am aiming to cover the various functions and capabilities of the dplyr and the tidyr packages.

With the recent Star Wars movie released, I want to use the starwars tibble for some analysis here!


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(tidyr)
data(starwars)
```

A tibble is quite different to a data frame in terms of printing or subsetting and it is simply a nicer way to create a data frame. It never adjusts the names of the variables of changes the input type. Check this article for more details.

Now let's view the dataset:



```r
starwars
```

```
## # A tibble: 87 x 13
##    name  height  mass hair_color skin_color eye_color birth_year gender
##    <chr>  <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
##  1 Luke…    172    77 blond      fair       blue            19   male  
##  2 C-3PO    167    75 <NA>       gold       yellow         112   <NA>  
##  3 R2-D2     96    32 <NA>       white, bl… red             33   <NA>  
##  4 Dart…    202   136 none       white      yellow          41.9 male  
##  5 Leia…    150    49 brown      light      brown           19   female
##  6 Owen…    178   120 brown, gr… light      blue            52   male  
##  7 Beru…    165    75 brown      light      blue            47   female
##  8 R5-D4     97    32 <NA>       white, red red             NA   <NA>  
##  9 Bigg…    183    84 black      light      brown           24   male  
## 10 Obi-…    182    77 auburn, w… fair       blue-gray       57   male  
## # … with 77 more rows, and 5 more variables: homeworld <chr>, species <chr>,
## #   films <list>, vehicles <list>, starships <list>
```

A tibble also displays just the top 10 rows and displays it on my Rstudio console.A dplyr equivalent for a data frame is the following command which also makes it easier to display the data


```r
dplyr::tbl_df(starwars)
```

```
## # A tibble: 87 x 13
##    name  height  mass hair_color skin_color eye_color birth_year gender
##    <chr>  <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
##  1 Luke…    172    77 blond      fair       blue            19   male  
##  2 C-3PO    167    75 <NA>       gold       yellow         112   <NA>  
##  3 R2-D2     96    32 <NA>       white, bl… red             33   <NA>  
##  4 Dart…    202   136 none       white      yellow          41.9 male  
##  5 Leia…    150    49 brown      light      brown           19   female
##  6 Owen…    178   120 brown, gr… light      blue            52   male  
##  7 Beru…    165    75 brown      light      blue            47   female
##  8 R5-D4     97    32 <NA>       white, red red             NA   <NA>  
##  9 Bigg…    183    84 black      light      brown           24   male  
## 10 Obi-…    182    77 auburn, w… fair       blue-gray       57   male  
## # … with 77 more rows, and 5 more variables: homeworld <chr>, species <chr>,
## #   films <list>, vehicles <list>, starships <list>
```

If we want to just glimpse the data we can use the following as well


```r
dplyr::glimpse(starwars)
```

```
## Observations: 87
## Variables: 13
## $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia O…
## $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, …
## $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77…
## $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", …
## $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", …
## $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue"…
## $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0,…
## $ gender     <chr> "male", NA, NA, "male", "female", "male", "female", NA, "m…
## $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "…
## $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Hum…
## $ films      <list> [<"Revenge of the Sith", "Return of the Jedi", "The Empir…
## $ vehicles   <list> [<"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "I…
## $ starships  <list> [<"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1…
```

Now one particular format that is commonly seen in dplyr code is the piping symbol which is %>%. It passes the object on the left hand side to the function on the right and makes the code more readable. We will see more of this later.

Now let's look at the different row-wise filtering options.


```r
dplyr::filter(starwars,mass<=20)
```

```
## # A tibble: 3 x 13
##   name  height  mass hair_color skin_color eye_color birth_year gender homeworld
##   <chr>  <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr>  <chr>    
## 1 Yoda      66    17 white      green      brown            896 male   <NA>     
## 2 Wick…     88    20 brown      brown      brown              8 male   Endor    
## 3 Ratt…     79    15 none       grey, blue unknown           NA male   Aleen Mi…
## # … with 4 more variables: species <chr>, films <list>, vehicles <list>,
## #   starships <list>
```

And I get what I wanted to see! Yoda's mass is 17, so the data is included in the results.

If you just want to view a set of rows say from 14 to 15, then we do the following:


```r
dplyr::slice(starwars, 13:14)
```

```
## # A tibble: 2 x 13
##   name  height  mass hair_color skin_color eye_color birth_year gender homeworld
##   <chr>  <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr>  <chr>    
## 1 Chew…    228   112 brown      unknown    blue             200 male   Kashyyyk 
## 2 Han …    180    80 brown      fair       brown             29 male   Corellia 
## # … with 4 more variables: species <chr>, films <list>, vehicles <list>,
## #   starships <list>
```

And look who we have here! Han Solo and Chewbacca.

Here is a column-wise manipulation:


```r
dplyr::select(starwars, name, species, birth_year)
```

```
## # A tibble: 87 x 3
##    name               species birth_year
##    <chr>              <chr>        <dbl>
##  1 Luke Skywalker     Human         19  
##  2 C-3PO              Droid        112  
##  3 R2-D2              Droid         33  
##  4 Darth Vader        Human         41.9
##  5 Leia Organa        Human         19  
##  6 Owen Lars          Human         52  
##  7 Beru Whitesun lars Human         47  
##  8 R5-D4              Droid         NA  
##  9 Biggs Darklighter  Human         24  
## 10 Obi-Wan Kenobi     Human         57  
## # … with 77 more rows
```

We can also do summary and grouping operations. For example if we want to see the average mass by gender, then:


```r
starwars %>%
group_by(gender) %>%
summarise(avg = mean(mass, na.rm=TRUE)) %>%
arrange(avg)
```

```
## # A tibble: 5 x 2
##   gender           avg
##   <chr>          <dbl>
## 1 <NA>            46.3
## 2 female          54.0
## 3 male            81.0
## 4 none           140  
## 5 hermaphrodite 1358
```

We can also create new variables. Say I want to create a BMI value. For the sake of simplicity let's ignore gender and set the formula to be weight / height(squared).


```r
starwars %>%
mutate(bmi = mass/(height^2)) %>%
arrange(desc(bmi)) %>%
select(name, species, height, mass, bmi)
```

```
## # A tibble: 87 x 5
##    name                  species        height  mass     bmi
##    <chr>                 <chr>           <int> <dbl>   <dbl>
##  1 Jabba Desilijic Tiure Hutt              175  1358 0.0443 
##  2 Dud Bolt              Vulptereen         94    45 0.00509
##  3 Yoda                  Yoda's species     66    17 0.00390
##  4 Owen Lars             Human             178   120 0.00379
##  5 IG-88                 Droid             200   140 0.0035 
##  6 R2-D2                 Droid              96    32 0.00347
##  7 Grievous              Kaleesh           216   159 0.00341
##  8 R5-D4                 Droid              97    32 0.00340
##  9 Jek Tono Porkins      Human             180   110 0.00340
## 10 Darth Vader           Human             202   136 0.00333
## # … with 77 more rows
```

We can also reshape some results using tidyr. Here is an example to append species and name.


```r
starwars %>%
unite(name_species, name, species) %>%
select(name_species)
```

```
## # A tibble: 87 x 1
##    name_species            
##    <chr>                   
##  1 Luke Skywalker_Human    
##  2 C-3PO_Droid             
##  3 R2-D2_Droid             
##  4 Darth Vader_Human       
##  5 Leia Organa_Human       
##  6 Owen Lars_Human         
##  7 Beru Whitesun lars_Human
##  8 R5-D4_Droid             
##  9 Biggs Darklighter_Human 
## 10 Obi-Wan Kenobi_Human    
## # … with 77 more rows
```

