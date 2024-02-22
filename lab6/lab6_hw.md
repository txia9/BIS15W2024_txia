---
title: "dplyr Superhero"
date: "2024-02-22"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. This lab doubles as your homework. Please complete the lab and push your final code to GitHub.  

## Load the libraries

```r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
superhero_powers <- read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here. Before you do anything, first have a look at the names of the variables. You can use `rename()` or `clean_names()`.    


```r
superhero_info_n <- superhero_info %>%
  clean_names() %>% 
  mutate_all(tolower)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, Alignment)
```

```
##  Alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

1. Who are the publishers of the superheros? Show the proportion of superheros from each publisher. Which publisher has the highest number of superheros?  


```r
table(superhero_info_n$publisher)
```

```
## 
##       abc studios dark horse comics         dc comics      george lucas 
##                 4                18               215                14 
##     hanna-barbera     harpercollins       icon comics    idw publishing 
##                 1                 6                 4                 4 
##      image comics     j. k. rowling  j. r. r. tolkien     marvel comics 
##                14                 1                 1               388 
##         microsoft      nbc - heroes         rebellion          shueisha 
##                 1                19                 1                 4 
##     sony pictures        south park         star trek              syfy 
##                 2                 1                 6                 5 
##      team epic tv       titan books universal studios         wildstorm 
##                 5                 1                 1                 3
```
DC Comics has the highest number of superheros.  

2. Notice that we have some neutral superheros! Who are they? List their names below.  


```r
superhero_info_n %>% 
  select(name, alignment) %>% 
  filter(alignment=="neutral")
```

```
## # A tibble: 24 × 2
##    name         alignment
##    <chr>        <chr>    
##  1 bizarro      neutral  
##  2 black flash  neutral  
##  3 captain cold neutral  
##  4 copycat      neutral  
##  5 deadpool     neutral  
##  6 deathstroke  neutral  
##  7 etrigan      neutral  
##  8 galactus     neutral  
##  9 gladiator    neutral  
## 10 indigo       neutral  
## # ℹ 14 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?


```r
select(superhero_info_n, "name", "alignment", "race")
```

```
## # A tibble: 734 × 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 a-bomb        good      human            
##  2 abe sapien    good      icthyo sapien    
##  3 abin sur      good      ungaran          
##  4 abomination   bad       human / radiation
##  5 abraxas       bad       cosmic entity    
##  6 absorbing man bad       human            
##  7 adam monroe   good      <NA>             
##  8 adam strange  good      human            
##  9 agent 13      good      <NA>             
## 10 agent bob     good      human            
## # ℹ 724 more rows
```

## Not Human
4. List all of the superheros that are not human.


```r
filter(superhero_info_n, race != "human")
```

```
## # A tibble: 222 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 abe … male   blue      icth… no hair    191    dark hor… blue       good     
##  2 abin… male   blue      unga… no hair    185    dc comics red        good     
##  3 abom… male   green     huma… no hair    203    marvel c… <NA>       bad      
##  4 abra… male   blue      cosm… black      <NA>   marvel c… <NA>       bad      
##  5 ajax  male   brown     cybo… black      193    marvel c… <NA>       bad      
##  6 alien male   <NA>      xeno… no hair    244    dark hor… black      bad      
##  7 amazo male   red       andr… <NA>       257    dc comics <NA>       bad      
##  8 angel male   <NA>      vamp… <NA>       <NA>   dark hor… <NA>       good     
##  9 ange… female yellow    muta… black      165    marvel c… <NA>       good     
## 10 anti… male   yellow    god … no hair    61     dc comics <NA>       bad      
## # ℹ 212 more rows
## # ℹ 1 more variable: weight <chr>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".


```r
good_guys <- superhero_info_n %>% 
  filter(alignment == "good")
```


```r
bad_guys <- filter(superhero_info_n, alignment == "bad")
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, race)
```

```
##               race   n     percent valid_percent
##              alien   3 0.006048387   0.010752688
##              alpha   5 0.010080645   0.017921147
##             amazon   2 0.004032258   0.007168459
##            android   4 0.008064516   0.014336918
##             animal   2 0.004032258   0.007168459
##          asgardian   3 0.006048387   0.010752688
##          atlantean   4 0.008064516   0.014336918
##         bolovaxian   1 0.002016129   0.003584229
##              clone   1 0.002016129   0.003584229
##             cyborg   3 0.006048387   0.010752688
##           demi-god   2 0.004032258   0.007168459
##              demon   3 0.006048387   0.010752688
##            eternal   1 0.002016129   0.003584229
##     flora colossus   1 0.002016129   0.003584229
##        frost giant   1 0.002016129   0.003584229
##      god / eternal   6 0.012096774   0.021505376
##             gungan   1 0.002016129   0.003584229
##              human 148 0.298387097   0.530465950
##    human / altered   2 0.004032258   0.007168459
##     human / cosmic   2 0.004032258   0.007168459
##  human / radiation   8 0.016129032   0.028673835
##         human-kree   2 0.004032258   0.007168459
##      human-spartoi   1 0.002016129   0.003584229
##       human-vulcan   1 0.002016129   0.003584229
##    human-vuldarian   1 0.002016129   0.003584229
##      icthyo sapien   1 0.002016129   0.003584229
##            inhuman   4 0.008064516   0.014336918
##    kakarantharaian   1 0.002016129   0.003584229
##         kryptonian   4 0.008064516   0.014336918
##            martian   1 0.002016129   0.003584229
##          metahuman   1 0.002016129   0.003584229
##             mutant  46 0.092741935   0.164874552
##     mutant / clone   1 0.002016129   0.003584229
##             planet   1 0.002016129   0.003584229
##             saiyan   1 0.002016129   0.003584229
##           symbiote   3 0.006048387   0.010752688
##           talokite   1 0.002016129   0.003584229
##         tamaranean   1 0.002016129   0.003584229
##            ungaran   1 0.002016129   0.003584229
##            vampire   2 0.004032258   0.007168459
##     yoda's species   1 0.002016129   0.003584229
##      zen-whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Vampires?

```r
filter(good_guys, race == "vampire")
```

```
## # A tibble: 2 × 10
##   name  gender eye_color race   hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr>  <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 angel male   <NA>      vampi… <NA>       <NA>   dark hor… <NA>       good     
## 2 blade male   brown     vampi… black      188    marvel c… <NA>       good     
## # ℹ 1 more variable: weight <chr>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad_guys$height <- as.numeric(bad_guys$height)
bad_guys %>% 
  filter(gender == "male") %>% 
  filter(height >= 200)
```

```
## # A tibble: 22 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
##  1 abom… male   green     huma… no hair       203 marvel c… <NA>       bad      
##  2 alien male   <NA>      xeno… no hair       244 dark hor… black      bad      
##  3 amazo male   red       andr… <NA>          257 dc comics <NA>       bad      
##  4 apoc… male   red       muta… black         213 marvel c… grey       bad      
##  5 bane  male   <NA>      human <NA>          203 dc comics <NA>       bad      
##  6 dark… male   red       new … no hair       267 dc comics grey       bad      
##  7 doct… male   brown     human brown         201 marvel c… <NA>       bad      
##  8 doct… male   brown     <NA>  brown         201 marvel c… <NA>       bad      
##  9 doom… male   red       alien white         244 dc comics <NA>       bad      
## 10 kill… male   red       meta… no hair       244 dc comics green      bad      
## # ℹ 12 more rows
## # ℹ 1 more variable: weight <chr>
```

9. Are there more good guys or bad guys with green hair?  

```r
green_hair_g <- filter(good_guys, hair_color == "green")
glimpse(green_hair_g)
```

```
## Rows: 7
## Columns: 10
## $ name       <chr> "beast boy", "captain planet", "doc samson", "hulk", "lyja"…
## $ gender     <chr> "male", "male", "male", "male", "female", "female", "female"
## $ eye_color  <chr> "green", "red", "blue", "green", "green", "green", "green"
## $ race       <chr> "human", "god / eternal", "human / radiation", "human / rad…
## $ hair_color <chr> "green", "green", "green", "green", "green", "green", "gree…
## $ height     <chr> "173", NA, "198", "244", NA, "170", "201"
## $ publisher  <chr> "dc comics", "marvel comics", "marvel comics", "marvel comi…
## $ skin_color <chr> "green", NA, NA, "green", NA, NA, NA
## $ alignment  <chr> "good", "good", "good", "good", "good", "good", "good"
## $ weight     <chr> "68", NA, "171", "630", NA, "52", "315"
```


```r
green_hair_b <- filter(bad_guys, hair_color== "green")
glimpse(green_hair_b)
```

```
## Rows: 1
## Columns: 10
## $ name       <chr> "joker"
## $ gender     <chr> "male"
## $ eye_color  <chr> "green"
## $ race       <chr> "human"
## $ hair_color <chr> "green"
## $ height     <dbl> 196
## $ publisher  <chr> "dc comics"
## $ skin_color <chr> "white"
## $ alignment  <chr> "bad"
## $ weight     <chr> "86"
```

There are more good guys with green hair color.  

10. Let's explore who the really small superheros are. In the `superhero_info` data, which have a weight less than 50? Be sure to sort your results by weight lowest to highest.  

```r
superhero_info_n$weight <- as.numeric(superhero_info_n$weight)
```


```r
superhero_info_n %>% 
  filter(weight <= 50) %>% 
  arrange(weight)
```

```
## # A tibble: 31 × 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 iron… male   blue      <NA>  no hair    <NA>   marvel c… <NA>       bad      
##  2 groot male   yellow    flor… <NA>       701    marvel c… <NA>       good     
##  3 jack… male   blue      human brown      71     dark hor… <NA>       good     
##  4 gala… male   black     cosm… black      876    marvel c… <NA>       neutral  
##  5 yoda  male   brown     yoda… white      66     george l… green      good     
##  6 fin … male   red       kaka… no hair    975    marvel c… green      good     
##  7 howa… male   brown     <NA>  yellow     79     marvel c… <NA>       good     
##  8 kryp… male   blue      kryp… white      64     dc comics <NA>       good     
##  9 rock… male   brown     anim… brown      122    marvel c… <NA>       good     
## 10 dash  male   blue      human blond      122    dark hor… <NA>       good     
## # ℹ 21 more rows
## # ℹ 1 more variable: weight <dbl>
```

11. Let's make a new variable that is the ratio of height to weight. Call this variable `height_weight_ratio`.    

```r
superhero_info %>% 
  mutate(height_weight_ratio = Height/Weight) %>% 
  arrange(desc(height_weight_ratio))
```

```
## # A tibble: 734 × 11
##    name      Gender `Eye color` Race  `Hair color` Height Publisher `Skin color`
##    <chr>     <chr>  <chr>       <chr> <chr>         <dbl> <chr>     <chr>       
##  1 Groot     Male   yellow      Flor… <NA>            701 Marvel C… <NA>        
##  2 Galactus  Male   black       Cosm… Black           876 Marvel C… <NA>        
##  3 Fin Fang… Male   red         Kaka… No Hair         975 Marvel C… green       
##  4 Longshot  Male   blue        Human Blond           188 Marvel C… <NA>        
##  5 Jack-Jack Male   blue        Human Brown            71 Dark Hor… <NA>        
##  6 Rocket R… Male   brown       Anim… Brown           122 Marvel C… <NA>        
##  7 Dash      Male   blue        Human Blond           122 Dark Hor… <NA>        
##  8 Howard t… Male   brown       <NA>  Yellow           79 Marvel C… <NA>        
##  9 Swarm     Male   yellow      Muta… No Hair         196 Marvel C… yellow      
## 10 Yoda      Male   brown       Yoda… White            66 George L… green       
## # ℹ 724 more rows
## # ℹ 3 more variables: Alignment <chr>, Weight <dbl>, height_weight_ratio <dbl>
```

12. Who has the highest height to weight ratio?  

Groot has the highest height to weight ratio. (I am Groot!).  

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

13. How many superheros have a combination of agility, stealth, super_strength, stamina?

```r
superhero_powers_n <- superhero_powers %>% 
  clean_names() %>% 
  mutate_all(tolower)
```


```r
superhero_powers_n %>% 
  select(agility, stealth, super_strength, stamina) %>% 
  filter(agility == "true" & stealth == "true" & super_strength =="true" & stamina == "true") %>% 
  glimpse()
```

```
## Rows: 40
## Columns: 4
## $ agility        <chr> "true", "true", "true", "true", "true", "true", "true",…
## $ stealth        <chr> "true", "true", "true", "true", "true", "true", "true",…
## $ super_strength <chr> "true", "true", "true", "true", "true", "true", "true",…
## $ stamina        <chr> "true", "true", "true", "true", "true", "true", "true",…
```
There are 40 superheroes have a combination of agility, stealth, super_strength, stamina. 

## Your Favorite
14. Pick your favorite superhero and let's see their powers!  


```r
superhero_powers_n %>% 
  filter(hero_names =="groot") %>% 
  select_if(all)
```

```
## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical
```

```
## # A tibble: 1 × 13
##   durability longevity intelligence super_strength duplication size_changing
##   <chr>      <chr>     <chr>        <chr>          <chr>       <chr>        
## 1 true       true      true         true           true        true         
## # ℹ 7 more variables: stamina <chr>, invulnerability <chr>,
## #   fire_resistance <chr>, regeneration <chr>, plant_control <chr>,
## #   matter_absorption <chr>, resurrection <chr>
```


15. Can you find your hero in the superhero_info data? Show their info!  

```r
filter(superhero_info_n, name == "groot")
```

```
## # A tibble: 1 × 10
##   name  gender eye_color race   hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr>  <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 groot male   yellow    flora… <NA>       701    marvel c… <NA>       good     
## # ℹ 1 more variable: weight <dbl>
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
