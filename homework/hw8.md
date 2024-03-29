---
title: "Homework 8"
author: "Ryan Chiang"
date: "2024-02-15"
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
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
sydneybeaches %>% glimpse()
```

```
## Rows: 3,690
## Columns: 8
## $ BeachId                   <dbl> 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, 25, …
## $ Region                    <chr> "Sydney City Ocean Beaches", "Sydney City Oc…
## $ Council                   <chr> "Randwick Council", "Randwick Council", "Ran…
## $ Site                      <chr> "Clovelly Beach", "Clovelly Beach", "Clovell…
## $ Longitude                 <dbl> 151.2675, 151.2675, 151.2675, 151.2675, 151.…
## $ Latitude                  <dbl> -33.91449, -33.91449, -33.91449, -33.91449, …
## $ Date                      <chr> "02/01/2013", "06/01/2013", "12/01/2013", "1…
## $ `Enterococci (cfu/100ml)` <dbl> 19, 3, 2, 13, 8, 7, 11, 97, 3, 0, 6, 0, 1, 8…
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library("here")
```

```
## here() starts at /Users/ryanchiang/Desktop/BIS15W2024_rchiang
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab9/data/sydneybeaches.csv")) %>% clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

```r
sydneybeaches
```

```
## # A tibble: 3,690 × 8
##    beach_id region  council site  longitude latitude date  enterococci_cfu_100ml
##       <dbl> <chr>   <chr>   <chr>     <dbl>    <dbl> <chr>                 <dbl>
##  1       25 Sydney… Randwi… Clov…      151.    -33.9 02/0…                    19
##  2       25 Sydney… Randwi… Clov…      151.    -33.9 06/0…                     3
##  3       25 Sydney… Randwi… Clov…      151.    -33.9 12/0…                     2
##  4       25 Sydney… Randwi… Clov…      151.    -33.9 18/0…                    13
##  5       25 Sydney… Randwi… Clov…      151.    -33.9 30/0…                     8
##  6       25 Sydney… Randwi… Clov…      151.    -33.9 05/0…                     7
##  7       25 Sydney… Randwi… Clov…      151.    -33.9 11/0…                    11
##  8       25 Sydney… Randwi… Clov…      151.    -33.9 23/0…                    97
##  9       25 Sydney… Randwi… Clov…      151.    -33.9 07/0…                     3
## 10       25 Sydney… Randwi… Clov…      151.    -33.9 25/0…                     0
## # ℹ 3,680 more rows
```
### The data is in long form, if we look at it by the tidyverse definition, yes 
3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>% 
  select(site, date, enterococci_cfu_100ml)
```

4. Pivot the data such that the dates are column names and each beach only appears once (wide format). Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches_long %>% 
  pivot_wider( names_from = date,
                values_from = enterococci_cfu_100ml)
```

5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>% 
  pivot_longer(
    -site,
    names_to = "date",
    values_to = "enterococci_cfu_100ml")
```

```
## # A tibble: 3,784 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ℹ 3,774 more rows
```

6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_long %>% 
  separate(date, into = c("day", "month", "year"), sep = "/")
```

```
## # A tibble: 3,690 × 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ℹ 3,680 more rows
```

7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
mean_ent_by_year_and_site_frame <- sydneybeaches_long %>% 
  separate(date, into = c("day", "month", "year"), sep = "/") %>% 
group_by(site, year) %>% 
summarise(meanent = mean(enterococci_cfu_100ml, na.rm = TRUE))
```

```
## `summarise()` has grouped output by 'site'. You can override using the
## `.groups` argument.
```

8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
wide_mean_ent_by_year_and_site_frame <- mean_ent_by_year_and_site_frame %>% 
  pivot_wider(values_from = meanent,
                names_from = year)
wide_mean_ent_by_year_and_site_frame
```

```
## # A tibble: 11 × 7
## # Groups:   site [11]
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2   22.9 
##  2 Bronte Beach             26.8   17.5   23.6    61.3  16.8   43.4 
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93  10.6 
##  4 Coogee Beach             39.7   52.6   40.3    59.5  20.7   21.6 
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7   17.6 
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2   59.1 
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8   38.0 
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6    9.21
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26  12.5 
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9  112.  
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4   15.5
```

9. What was the most polluted beach in 2013?


```r
mean_ent_by_year_and_site_frame %>% 
  filter(year == "2013") %>% 
  arrange(desc(meanent))
```

```
## # A tibble: 11 × 3
## # Groups:   site [11]
##    site                    year  meanent
##    <chr>                   <chr>   <dbl>
##  1 Little Bay Beach        2013   122.  
##  2 Malabar Beach           2013   101.  
##  3 South Maroubra Rockpool 2013    96.4 
##  4 Maroubra Beach          2013    47.1 
##  5 Coogee Beach            2013    39.7 
##  6 South Maroubra Beach    2013    39.3 
##  7 Bondi Beach             2013    32.2 
##  8 Tamarama Beach          2013    29.7 
##  9 Bronte Beach            2013    26.8 
## 10 Gordons Bay (East)      2013    24.8 
## 11 Clovelly Beach          2013     9.28
```

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
