---
title: "Lab 2 Homework"
author: "Ryan Chiang"
date: "2024-01-18"
output:
  html_document: 
    theme: spacelab
    keep_md: true
  pdf_document:
    toc: false
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  
A vector is a list of variables

2. What is a data matrix in R?  
A data matrix is a collection of data aranged in rows and columns.

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

```r
springdata_collected <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
spring_data_matrix <- matrix(springdata_collected, nrow = 8, byrow = T)
spring_data_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```


5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.



```r
Spring_name <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", "Too Hot Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")
Scientist <- c("Jill", "Steve", "Susan")
colnames(spring_data_matrix) <- Scientist
row.names(spring_data_matrix) <- Spring_name
spring_data_matrix
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## Too Hot Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```



6. Calculate the mean temperature of all eight springs.

```r
rowMeans(spring_data_matrix)
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##         35.65000         34.61667         29.85000         39.46667 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##         30.85000         30.20000         32.73333         35.46667
```


7. Add this as a new column in the data matrix.  

```r
Avg_temp  <- rowMeans(spring_data_matrix)
spring_data_matrix_with_avgs <- cbind(spring_data_matrix, Avg_temp)
spring_data_matrix_with_avgs
```

```
##                   Jill Steve Susan Avg_temp
## Bluebell Spring  36.25 35.40 35.30 35.65000
## Opal Spring      35.15 35.35 33.35 34.61667
## Riverside Spring 30.70 29.65 29.20 29.85000
## Too Hot Spring   39.70 40.05 38.65 39.46667
## Mystery Spring   31.85 31.40 29.30 30.85000
## Emerald Spring   30.20 30.65 29.75 30.20000
## Black Spring     32.90 32.50 32.80 32.73333
## Pearl Spring     36.80 36.45 33.15 35.46667
```

8. Show Susan's value for Opal Spring only.


```r
spring_data_matrix_with_avgs[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
spring_data_matrix_with_avgs[1:8]
```

```
## [1] 36.25 35.15 30.70 39.70 31.85 30.20 32.90 36.80
```

```r
mean(spring_data_matrix_with_avgs[1:8])
```

```
## [1] 34.19375
```


10. Use the data matrix to perform one calculation or operation of your interest.


```r
rowSums(spring_data_matrix)
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##           106.95           103.85            89.55           118.40 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##            92.55            90.60            98.20           106.40
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
