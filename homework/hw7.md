---
title: "Homework 7"
author: "Your Name Here"
date: "2024-02-11"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
library(naniar)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <- read.csv("data/amniota.csv")
amniota <- clean_names(amniota)
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- read.csv("data/amphibio.csv")
amphibio <- clean_names(amphibio)
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

```r
amniota %>% 
  glimpse() %>% 
  map_df(~sum(is.na(.)))
```

```
## Rows: 21,322
## Columns: 36
## $ class                                 <chr> "Aves", "Aves", "Aves", "Aves", …
## $ order                                 <chr> "Accipitriformes", "Accipitrifor…
## $ family                                <chr> "Accipitridae", "Accipitridae", …
## $ genus                                 <chr> "Accipiter", "Accipiter", "Accip…
## $ species                               <chr> "albogularis", "badius", "bicolo…
## $ subspecies                            <int> -999, -999, -999, -999, -999, -9…
## $ common_name                           <chr> "Pied Goshawk", "Shikra", "Bicol…
## $ female_maturity_d                     <dbl> -999.000, 363.468, -999.000, -99…
## $ litter_or_clutch_size_n               <dbl> -999.000, 3.250, 2.700, -999.000…
## $ litters_or_clutches_per_y             <dbl> -999, 1, -999, -999, 1, -999, -9…
## $ adult_body_mass_g                     <dbl> 251.500, 140.000, 345.000, 142.0…
## $ maximum_longevity_y                   <dbl> -999.00000, -999.00000, -999.000…
## $ gestation_d                           <dbl> -999, -999, -999, -999, -999, -9…
## $ weaning_d                             <dbl> -999, -999, -999, -999, -999, -9…
## $ birth_or_hatching_weight_g            <dbl> -999, -999, -999, -999, -999, -9…
## $ weaning_weight_g                      <dbl> -999, -999, -999, -999, -999, -9…
## $ egg_mass_g                            <dbl> -999.00, 21.00, 32.00, -999.00, …
## $ incubation_d                          <dbl> -999.00, 30.00, -999.00, -999.00…
## $ fledging_age_d                        <dbl> -999.00, 32.00, -999.00, -999.00…
## $ longevity_y                           <dbl> -999.00000, -999.00000, -999.000…
## $ male_maturity_d                       <dbl> -999, -999, -999, -999, -999, -9…
## $ inter_litter_or_interbirth_interval_y <dbl> -999, -999, -999, -999, -999, -9…
## $ female_body_mass_g                    <dbl> 352.500, 168.500, 390.000, -999.…
## $ male_body_mass_g                      <dbl> 223.000, 125.000, 212.000, 142.0…
## $ no_sex_body_mass_g                    <dbl> -999.0, 123.0, -999.0, -999.0, -…
## $ egg_width_mm                          <dbl> -999, -999, -999, -999, -999, -9…
## $ egg_length_mm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ fledging_mass_g                       <dbl> -999, -999, -999, -999, -999, -9…
## $ adult_svl_cm                          <dbl> -999.00, 30.00, 39.50, -999.00, …
## $ male_svl_cm                           <dbl> -999, -999, -999, -999, -999, -9…
## $ female_svl_cm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ birth_or_hatching_svl_cm              <dbl> -999, -999, -999, -999, -999, -9…
## $ female_svl_at_maturity_cm             <dbl> -999, -999, -999, -999, -999, -9…
## $ female_body_mass_at_maturity_g        <int> -999, -999, -999, -999, -999, -9…
## $ no_sex_svl_cm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ no_sex_maturity_d                     <dbl> -999, -999, -999, -999, -999, -9…
```

```
## # A tibble: 1 × 36
##   class order family genus species subspecies common_name female_maturity_d
##   <int> <int>  <int> <int>   <int>      <int>       <int>             <int>
## 1     0     0      0     0       0          0           0                 0
## # ℹ 28 more variables: litter_or_clutch_size_n <int>,
## #   litters_or_clutches_per_y <int>, adult_body_mass_g <int>,
## #   maximum_longevity_y <int>, gestation_d <int>, weaning_d <int>,
## #   birth_or_hatching_weight_g <int>, weaning_weight_g <int>, egg_mass_g <int>,
## #   incubation_d <int>, fledging_age_d <int>, longevity_y <int>,
## #   male_maturity_d <int>, inter_litter_or_interbirth_interval_y <int>,
## #   female_body_mass_g <int>, male_body_mass_g <int>, …
```

**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

```r
amphibio %>% 
  glimpse() %>% 
  map_df(~sum(is.na(.)))
```

```
## Rows: 6,776
## Columns: 38
## $ id                      <chr> "Anf0001", "Anf0002", "Anf0003", "Anf0004", "A…
## $ order                   <chr> "Anura", "Anura", "Anura", "Anura", "Anura", "…
## $ family                  <chr> "Allophrynidae", "Alytidae", "Alytidae", "Alyt…
## $ genus                   <chr> "Allophryne", "Alytes", "Alytes", "Alytes", "A…
## $ species                 <chr> "Allophryne ruthveni", "Alytes cisternasii", "…
## $ fos                     <int> NA, NA, NA, NA, NA, 1, 1, 1, 1, 1, 1, 1, 1, NA…
## $ ter                     <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
## $ aqu                     <int> 1, 1, 1, 1, NA, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ arb                     <int> 1, 1, 1, 1, 1, 1, NA, NA, NA, NA, NA, NA, NA, …
## $ leaves                  <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ flowers                 <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ seeds                   <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ fruits                  <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ arthro                  <int> 1, 1, 1, NA, 1, 1, 1, 1, 1, NA, 1, 1, NA, NA, …
## $ vert                    <int> NA, NA, NA, NA, NA, NA, 1, NA, NA, NA, 1, 1, N…
## $ diu                     <int> 1, NA, NA, NA, NA, NA, 1, 1, 1, NA, 1, 1, NA, …
## $ noc                     <int> 1, 1, 1, NA, 1, 1, 1, 1, 1, NA, 1, 1, 1, NA, N…
## $ crepu                   <int> 1, NA, NA, NA, NA, 1, NA, NA, NA, NA, NA, NA, …
## $ wet_warm                <int> NA, NA, NA, NA, 1, 1, NA, NA, NA, NA, 1, NA, N…
## $ wet_cold                <int> 1, NA, NA, NA, NA, NA, 1, NA, NA, NA, NA, NA, …
## $ dry_warm                <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ dry_cold                <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ body_mass_g             <dbl> 31.00, 6.10, NA, NA, 2.31, 13.40, 21.80, NA, N…
## $ age_at_maturity_min_y   <dbl> NA, 2.0, 2.0, NA, 3.0, 2.0, 3.0, NA, NA, NA, 4…
## $ age_at_maturity_max_y   <dbl> NA, 2.0, 2.0, NA, 3.0, 3.0, 5.0, NA, NA, NA, 4…
## $ body_size_mm            <dbl> 31.0, 50.0, 55.0, NA, 40.0, 55.0, 80.0, 60.0, …
## $ size_at_maturity_min_mm <dbl> NA, 27, NA, NA, NA, 35, NA, NA, NA, NA, NA, NA…
## $ size_at_maturity_max_mm <dbl> NA, 36.0, NA, NA, NA, 40.5, NA, NA, NA, NA, NA…
## $ longevity_max_y         <dbl> NA, 6, NA, NA, NA, 7, 9, NA, NA, NA, NA, NA, N…
## $ litter_size_min_n       <dbl> 300, 60, 40, NA, 7, 53, 300, 1500, 1000, NA, 2…
## $ litter_size_max_n       <dbl> 300, 180, 40, NA, 20, 171, 1500, 1500, 1000, N…
## $ reproductive_output_y   <dbl> 1, 4, 1, 4, 1, 4, 6, 1, 1, 1, 1, 1, 1, 1, NA, …
## $ offspring_size_min_mm   <dbl> NA, 2.6, NA, NA, 5.4, 2.6, 1.5, NA, 1.5, NA, 1…
## $ offspring_size_max_mm   <dbl> NA, 3.5, NA, NA, 7.0, 5.0, 2.0, NA, 1.5, NA, 1…
## $ dir                     <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, N…
## $ lar                     <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, N…
## $ viv                     <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, N…
## $ obs                     <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
```

```
## # A tibble: 1 × 38
##      id order family genus species   fos   ter   aqu   arb leaves flowers seeds
##   <int> <int>  <int> <int>   <int> <int> <int> <int> <int>  <int>   <int> <int>
## 1     0     0      0     0       0  6053  1104  2810  4347   6752    6772  6772
## # ℹ 26 more variables: fruits <int>, arthro <int>, vert <int>, diu <int>,
## #   noc <int>, crepu <int>, wet_warm <int>, wet_cold <int>, dry_warm <int>,
## #   dry_cold <int>, body_mass_g <int>, age_at_maturity_min_y <int>,
## #   age_at_maturity_max_y <int>, body_size_mm <int>,
## #   size_at_maturity_min_mm <int>, size_at_maturity_max_mm <int>,
## #   longevity_max_y <int>, litter_size_min_n <int>, litter_size_max_n <int>,
## #   reproductive_output_y <int>, offspring_size_min_mm <int>, …
```

**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   

```r
amphibio %>% 
  map_df(~sum(is.na(.))) %>% 
  sum()
```

```
## [1] 170564
```

```r
amniota %>% 
  map_df(~sum(is.na(.))) %>% 
  sum()
```

```
## [1] 0
```
### the value for amphibio makes sense the value for amniota does not
**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota <- amniota %>% 
 replace_with_na_all(condition = ~.x == -999)
```
### code chunk still did not work
**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
miss_var_summary(amniota)
```

```
## # A tibble: 36 × 3
##    variable                       n_miss pct_miss
##    <chr>                           <int>    <dbl>
##  1 subspecies                      21322    100  
##  2 female_body_mass_at_maturity_g  21318    100. 
##  3 female_svl_at_maturity_cm       21120     99.1
##  4 fledging_mass_g                 21111     99.0
##  5 male_svl_cm                     21040     98.7
##  6 no_sex_maturity_d               20860     97.8
##  7 egg_width_mm                    20727     97.2
##  8 egg_length_mm                   20702     97.1
##  9 weaning_weight_g                20258     95.0
## 10 female_svl_cm                   20242     94.9
## # ℹ 26 more rows
```

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
miss_var_summary(amphibio)
```

```
## # A tibble: 38 × 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 fruits     6774    100. 
##  2 flowers    6772     99.9
##  3 seeds      6772     99.9
##  4 leaves     6752     99.6
##  5 dry_cold   6735     99.4
##  6 vert       6657     98.2
##  7 obs        6649     98.1
##  8 wet_cold   6625     97.8
##  9 crepu      6608     97.5
## 10 dry_warm   6572     97.0
## # ℹ 28 more rows
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  

```r
amniota %>% 
  group_by(class) %>% 
  select(class, egg_mass_g) %>% 
  miss_var_summary(order = T)
```

```
## # A tibble: 3 × 4
## # Groups:   class [3]
##   class    variable   n_miss pct_miss
##   <chr>    <chr>       <int>    <dbl>
## 1 Aves     egg_mass_g   4914     50.1
## 2 Mammalia egg_mass_g   4953    100  
## 3 Reptilia egg_mass_g   6040     92.0
```
### It makes sense that the percent of NAs in egg mass in grams for mammals is 100% because mammals don't lay eggs. This changes my interpretation of NAs because NA is not just used for non recorded data but also as a placeholder data that does not pertain to a particular subject. 

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

```r
amphibio %>% 
  select(arb, fos, aqu, ter) %>% 
  miss_var_summary(order = T)
```

```
## # A tibble: 4 × 3
##   variable n_miss pct_miss
##   <chr>     <int>    <dbl>
## 1 fos        6053     89.3
## 2 arb        4347     64.2
## 3 aqu        2810     41.5
## 4 ter        1104     16.3
```
### I believe the author intended to use numeric approach by using 1 and NA to indicate weather an organism has a lifesytle feature, instead of using a character data class (TRUE/FALSE). 
**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

```r
amniota_no_nas <-
read.csv(file ="data/amniota.csv", na = c("NA", "-999", "-999.00")) %>% 
  clean_names()
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
