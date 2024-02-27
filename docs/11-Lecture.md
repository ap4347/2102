---
output:
  html_document: default
  pdf_document: default
---
# Lecture 11 {-}

&nbsp;


## dplyr Package (continued) {-}


We continue working with the `nyc_flights` data set that provides information about flights departed New York City in 2013 (the data set is available on Courseworks). It contains 336 776 observations (rows) and 19 variables (columns). 





### group_by(), summarise(), and across() Functions {-}

Most data operations are done on groups defined by variables. `dplyr` verbs are particularly powerful when you apply them to grouped data frames. The most important grouping verb is `group_by()`. It takes an existing data frame and converts it into a grouped data frame where operations are performed "by group". In other words, it takes a data frame and one or more variables to group by:


```r

by_origin <- flights %>% group_by(origin)

by_origin
#> # A tibble: 336,776 × 19
#> # Groups:   origin [3]
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013     1     1      517            515         2
#>  2  2013     1     1      533            529         4
#>  3  2013     1     1      542            540         2
#>  4  2013     1     1      544            545        -1
#>  5  2013     1     1      554            600        -6
#>  6  2013     1     1      554            558        -4
#>  7  2013     1     1      555            600        -5
#>  8  2013     1     1      557            600        -3
#>  9  2013     1     1      557            600        -3
#> 10  2013     1     1      558            600        -2
#> # ℹ 336,766 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <fct>,
#> #   flight <int>, tailnum <fct>, origin <fct>, dest <fct>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <fct>
```


```r

by_origin_carrier <- flights %>% group_by(origin, carrier)

by_origin_carrier
#> # A tibble: 336,776 × 19
#> # Groups:   origin, carrier [35]
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013     1     1      517            515         2
#>  2  2013     1     1      533            529         4
#>  3  2013     1     1      542            540         2
#>  4  2013     1     1      544            545        -1
#>  5  2013     1     1      554            600        -6
#>  6  2013     1     1      554            558        -4
#>  7  2013     1     1      555            600        -5
#>  8  2013     1     1      557            600        -3
#>  9  2013     1     1      557            600        -3
#> 10  2013     1     1      558            600        -2
#> # ℹ 336,766 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <fct>,
#> #   flight <int>, tailnum <fct>, origin <fct>, dest <fct>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <fct>
```


Grouping does not change how the data looks apart from listing how it is grouped.

Grouping is most useful when used in conjunction with the `summarise()` function. `summarise()` creates a new data frame. It returns one row for each combination of grouping variables; if there are no grouping variables, the output will have a single row summarizing all observations in the input. It will contain one column for each grouping variable and one column for each of the summary statistics that you have specified. Thus, it changes the unit of analysis from the complete dataset to individual groups. Together `group_by()` and `summarise()` provide one of the tools that you’ll use most commonly when working with dplyr: grouped summaries.

For instance, let's calculate the average arrival delay time for each group in the `by_origin` grouped data:


```r

by_origin %>% summarise(Mean = mean(arr_delay, na.rm = T))
#> # A tibble: 3 × 2
#>   origin  Mean
#>   <fct>  <dbl>
#> 1 EWR     9.11
#> 2 JFK     5.55
#> 3 LGA     5.78
```

You can even pass several variables to it:


```r

by_origin %>% 
  
  summarise(Mean = mean(arr_delay, na.rm = T),
            
            Median = median(arr_delay, na.rm = T),
            
            Count = n())
#> # A tibble: 3 × 4
#>   origin  Mean Median  Count
#>   <fct>  <dbl>  <dbl>  <int>
#> 1 EWR     9.11     -4 120835
#> 2 JFK     5.55     -6 111279
#> 3 LGA     5.78     -5 104662
```


The table below displays useful functions that are frequently used with `summarise()`:


Functionality | Functions
--------------| -----------
Center | `mean()`, `median()`
Spread | `sd()`, `IQR()`, `var()`, `quantile()`
Range | `min()`, `max()`, `range()`
Position| `first()`, `last()`, `nth()`
Count | `n()`, `n_distinct()`
Logical | `any()`, `all()`


`group_by()` and `summarise()` functions can be combined with other single table verbs:



```r

by_carrier <- flights %>% group_by(carrier)

by_carrier %>%
  
  summarise (Count = n(), Distance_sd = sd(distance)) %>%
  
  filter(Count < 10000) %>%
  
  arrange(desc(Distance_sd))
#> # A tibble: 7 × 3
#>   carrier Count Distance_sd
#>   <fct>   <int>       <dbl>
#> 1 OO         32       206. 
#> 2 FL       3260       161. 
#> 3 YV        601       160. 
#> 4 VX       5162        88.0
#> 5 AS        714         0  
#> 6 F9        685         0  
#> 7 HA        342         0
```


If you need to remove grouping and return to operations on ungrouped data, use `ungroup()`:


```r

by_carrier %>%
  
  ungroup() %>%
  
  summarise(flights = n())
#> # A tibble: 1 × 1
#>   flights
#>     <int>
#> 1  336776
```


It’s often useful to perform the same operation on multiple columns, but copying and pasting is both tedious and error prone. For example:



```r

flights %>% 
  
  group_by(origin, carrier) %>%
  
  summarise(Mean_dep_delay = mean(dep_delay, na.rm = T),
            
            Mean_arrival_delay = mean(arr_delay, na.rm = T),
            
            Mean_air_time = mean(air_time, na.rm = T))
#> `summarise()` has grouped output by 'origin'. You can
#> override using the `.groups` argument.
#> # A tibble: 35 × 5
#> # Groups:   origin [3]
#>    origin carrier Mean_dep_delay Mean_arrival_delay
#>    <fct>  <fct>            <dbl>              <dbl>
#>  1 EWR    9E                5.95              1.62 
#>  2 EWR    AA               10.0               0.978
#>  3 EWR    AS                5.80             -9.93 
#>  4 EWR    B6               13.1               9.39 
#>  5 EWR    DL               12.1               8.78 
#>  6 EWR    EV               20.2              17.0  
#>  7 EWR    MQ               17.5              16.3  
#>  8 EWR    OO               20.8              21.5  
#>  9 EWR    UA               12.5               3.48 
#> 10 EWR    US                3.74              0.977
#> # ℹ 25 more rows
#> # ℹ 1 more variable: Mean_air_time <dbl>
```


Instead, we can use `across()` function, which  lets you rewrite the previous code more succinctly:


```r

flights %>% 
  
  group_by(origin, carrier) %>%
  
  summarise(across(
    
    c(dep_delay, arr_delay, air_time),
    
    ~ mean(.x, na.rm = T)
    
  ))
#> `summarise()` has grouped output by 'origin'. You can
#> override using the `.groups` argument.
#> # A tibble: 35 × 5
#> # Groups:   origin [3]
#>    origin carrier dep_delay arr_delay air_time
#>    <fct>  <fct>       <dbl>     <dbl>    <dbl>
#>  1 EWR    9E           5.95     1.62     103. 
#>  2 EWR    AA          10.0      0.978    196. 
#>  3 EWR    AS           5.80    -9.93     326. 
#>  4 EWR    B6          13.1      9.39     118. 
#>  5 EWR    DL          12.1      8.78     125. 
#>  6 EWR    EV          20.2     17.0       94.0
#>  7 EWR    MQ          17.5     16.3      112. 
#>  8 EWR    OO          20.8     21.5      137. 
#>  9 EWR    UA          12.5      3.48     207. 
#> 10 EWR    US           3.74     0.977    138. 
#> # ℹ 25 more rows
```


`across()` has two primary arguments: (1) the first argument, `.col`, selects the columns you want to operate on; (2) the second argument, `.fns`, is a function or list of functions to apply to each column. Here are some examples:


```r

flights %>% 
  
  summarise(across(where(is.factor), n_distinct))
#> # A tibble: 1 × 5
#>   carrier tailnum origin  dest time_hour
#>     <int>   <int>  <int> <int>     <int>
#> 1      16    4044      3   105      6936
```



```r

flights %>% 
  
  group_by(origin) %>%
  
  summarise(across(where(is.numeric), ~ mean(.x, na.rm = TRUE)))
#> # A tibble: 3 × 15
#>   origin  year month   day dep_time sched_dep_time dep_delay
#>   <fct>  <dbl> <dbl> <dbl>    <dbl>          <dbl>     <dbl>
#> 1 EWR     2013  6.49  15.7    1337.          1322.      15.1
#> 2 JFK     2013  6.50  15.7    1399.          1402.      12.1
#> 3 LGA     2013  6.67  15.7    1310.          1308.      10.3
#> # ℹ 8 more variables: arr_time <dbl>, sched_arr_time <dbl>,
#> #   arr_delay <dbl>, flight <dbl>, air_time <dbl>,
#> #   distance <dbl>, hour <dbl>, minute <dbl>
```


You can transform each variable with more than one function by supplying a named list of functions or lambda functions in the second argument:


```r


min_max <- list(
  
  min = ~min(.x, na.rm = TRUE), 
  
  max = ~max(.x, na.rm = TRUE)
)

flights %>% 
  
  group_by(origin) %>%
  
  summarise(across(where(is.numeric), min_max))
#> # A tibble: 3 × 29
#>   origin year_min year_max month_min month_max day_min
#>   <fct>     <int>    <int>     <int>     <int>   <int>
#> 1 EWR        2013     2013         1        12       1
#> 2 JFK        2013     2013         1        12       1
#> 3 LGA        2013     2013         1        12       1
#> # ℹ 23 more variables: day_max <int>, dep_time_min <int>,
#> #   dep_time_max <int>, sched_dep_time_min <int>,
#> #   sched_dep_time_max <int>, dep_delay_min <int>,
#> #   dep_delay_max <int>, arr_time_min <int>,
#> #   arr_time_max <int>, sched_arr_time_min <int>,
#> #   sched_arr_time_max <int>, arr_delay_min <int>,
#> #   arr_delay_max <int>, flight_min <int>, …
```

You can control how the names are created with the `.names` argument:


```r

flights %>% 
  
  group_by(origin) %>%
  
  summarise(across(where(is.numeric), min_max,
                   
                   .names = "{.fn}.{.col}"
                   
                   ))
#> # A tibble: 3 × 29
#>   origin min.year max.year min.month max.month min.day
#>   <fct>     <int>    <int>     <int>     <int>   <int>
#> 1 EWR        2013     2013         1        12       1
#> 2 JFK        2013     2013         1        12       1
#> 3 LGA        2013     2013         1        12       1
#> # ℹ 23 more variables: max.day <int>, min.dep_time <int>,
#> #   max.dep_time <int>, min.sched_dep_time <int>,
#> #   max.sched_dep_time <int>, min.dep_delay <int>,
#> #   max.dep_delay <int>, min.arr_time <int>,
#> #   max.arr_time <int>, min.sched_arr_time <int>,
#> #   max.sched_arr_time <int>, min.arr_delay <int>,
#> #   max.arr_delay <int>, min.flight <int>, …
```


## Relational Data: Two-Table Verbs {-}

It’s rare that a data analysis involves only a single table of data. In practice, you’ll normally have many tables that contribute to an analysis, and you need flexible tools to combine them. 


In dplyr, there are three families of verbs that work with two tables at a time:

* **Mutating joins**, which add new variables to one table from matching rows in another
* **Filtering joins**, which filter observations from one table based on whether or not they match an observation in the other table
* **Set operations**, which combine the observations in the data sets as if they were set elements


### Mutating joins {-}

Mutating joins allow you to combine variables from multiple tables. It first matches observations by their keys, then copies across variables from one table to the other. There are four types of mutating join, which differ in their behavior when a match is not found. These are:

* `inner_join()`
* `left_join()`
* `right_join()`
* `full_join()`

All these functions have the same input arguments. We will be focusing on the following arguments:

* `x` and `y` - tables or dataframes that are being combined (`x` is known as a primary table and `y` as a secondary table)
* `by` - the join key, a variable or variables that is/are used to match the rows between the `x` and `y` tables. In other words, it controls which variables are used to match observations in the two tables.
* `keep` - a logical operator indicating whether the join keys from both `x` and `y` tables should be preserved in the output. The default value is `FALSE`.


The output is always a new table. By default, if an observation in `x` matches multiple observations in `y`, all of the matching observations in `y` will be returned. If this occurs, normally a warning will be thrown stating that multiple matches have been detected since this is usually surprising.

To illustrative how these functions work, we will be using the following toy data frames:


```r

df1 <- data.frame(
  
  a = c(1, 2, 3, 2, 4),
  
  b = c(10, 20, 30, 35, 40),
  
  c  = c(100, 200, 300, 350, 400)
  
)

print(df1)
#>   a  b   c
#> 1 1 10 100
#> 2 2 20 200
#> 3 3 30 300
#> 4 2 35 350
#> 5 4 40 400
```



```r

df2 <- data.frame(
  
  a =  c(1, 2, 5, 4, 6, 2),
  
  b  =  c(10, 40, 50, 40, 60, 50),
  
  x = c(15, 25, 35, 45, 55, 65),
  
  z = c(150, 200, 350, 400, 550, 270)

)

print(df2)
#>   a  b  x   z
#> 1 1 10 15 150
#> 2 2 40 25 200
#> 3 5 50 35 350
#> 4 4 40 45 400
#> 5 6 60 55 550
#> 6 2 50 65 270
```


#### inner_join() {-}

The simplest type of join is the **inner join**. An inner join matches pairs of observations whenever their keys are equal. The output of an inner join is a new data frame that contains the key, the `x` values, and the `y` values. The most important property of an inner join is that unmatched rows in either input are not included in the result.

Below are some examples of inner join:


```r

# Merging tables by the "a" variable

df1 %>%
  
  inner_join(df2, by = "a")
#> Warning in inner_join(., df2, by = "a"): Detected an unexpected many-to-many relationship between
#> `x` and `y`.
#> ℹ Row 2 of `x` matches multiple rows in `y`.
#> ℹ Row 2 of `y` matches multiple rows in `x`.
#> ℹ If a many-to-many relationship is expected, set
#>   `relationship = "many-to-many"` to silence this warning.
#>   a b.x   c b.y  x   z
#> 1 1  10 100  10 15 150
#> 2 2  20 200  40 25 200
#> 3 2  20 200  50 65 270
#> 4 2  35 350  40 25 200
#> 5 2  35 350  50 65 270
#> 6 4  40 400  40 45 400
```



```r

# Merging tables by the "a" and "b" variable

df1 %>%
  
  inner_join(df2, by = c("a", "b"))
#>   a  b   c  x   z
#> 1 1 10 100 15 150
#> 2 4 40 400 45 400
```



```r

## Merging tables by the "c" and "z" variable (Have different variable names)

df1 %>%
  
  inner_join(df2, by = c("c" = "z"))
#>   a.x b.x   c a.y b.y  x
#> 1   2  20 200   2  40 25
#> 2   2  35 350   5  50 35
#> 3   4  40 400   4  40 45
```



```r

## Merging tables by the "c" and "z" variable (Have different variable names) and keeping both key variables in the output table

df1 %>%
  
  inner_join(df2, by = c("c" = "z"), keep = T)
#>   a.x b.x   c a.y b.y  x   z
#> 1   2  20 200   2  40 25 200
#> 2   2  35 350   5  50 35 350
#> 3   4  40 400   4  40 45 400
```


#### left_join() {-}

`left_join()` includes all observations in `x`, regardless of whether they match or not. This is the most commonly used join because it ensures that you don’t lose observations from your primary table:


```r

# Merging tables by the "a" variable

df1 %>%
  
  left_join(df2, by = "a")
#> Warning in left_join(., df2, by = "a"): Detected an unexpected many-to-many relationship between
#> `x` and `y`.
#> ℹ Row 2 of `x` matches multiple rows in `y`.
#> ℹ Row 2 of `y` matches multiple rows in `x`.
#> ℹ If a many-to-many relationship is expected, set
#>   `relationship = "many-to-many"` to silence this warning.
#>   a b.x   c b.y  x   z
#> 1 1  10 100  10 15 150
#> 2 2  20 200  40 25 200
#> 3 2  20 200  50 65 270
#> 4 3  30 300  NA NA  NA
#> 5 2  35 350  40 25 200
#> 6 2  35 350  50 65 270
#> 7 4  40 400  40 45 400
```



```r

# Merging tables by the "a" and "b" variable

df1 %>%
  
  left_join(df2, by = c("a", "b"))
#>   a  b   c  x   z
#> 1 1 10 100 15 150
#> 2 2 20 200 NA  NA
#> 3 3 30 300 NA  NA
#> 4 2 35 350 NA  NA
#> 5 4 40 400 45 400
```



```r

## Merging tables by the "c" and "z" variable (Have different variable names)

df1 %>%
  
  left_join(df2, by = c("c" = "z"))
#>   a.x b.x   c a.y b.y  x
#> 1   1  10 100  NA  NA NA
#> 2   2  20 200   2  40 25
#> 3   3  30 300  NA  NA NA
#> 4   2  35 350   5  50 35
#> 5   4  40 400   4  40 45
```



#### right_join() {-}

`right_join()` includes all observations in `y`:


```r

# Merging tables by the "a" variable

df1 %>%
  
  right_join(df2, by = "a")
#> Warning in right_join(., df2, by = "a"): Detected an unexpected many-to-many relationship between
#> `x` and `y`.
#> ℹ Row 2 of `x` matches multiple rows in `y`.
#> ℹ Row 2 of `y` matches multiple rows in `x`.
#> ℹ If a many-to-many relationship is expected, set
#>   `relationship = "many-to-many"` to silence this warning.
#>   a b.x   c b.y  x   z
#> 1 1  10 100  10 15 150
#> 2 2  20 200  40 25 200
#> 3 2  20 200  50 65 270
#> 4 2  35 350  40 25 200
#> 5 2  35 350  50 65 270
#> 6 4  40 400  40 45 400
#> 7 5  NA  NA  50 35 350
#> 8 6  NA  NA  60 55 550
```



```r

# Merging tables by the "a" and "b" variable

df1 %>%
  
  right_join(df2, by = c("a", "b"))
#>   a  b   c  x   z
#> 1 1 10 100 15 150
#> 2 4 40 400 45 400
#> 3 2 40  NA 25 200
#> 4 5 50  NA 35 350
#> 5 6 60  NA 55 550
#> 6 2 50  NA 65 270
```



```r

## Merging tables by the "c" and "z" variable (Have different variable names)

df1 %>%
  
  right_join(df2, by = c("c" = "z"))
#>   a.x b.x   c a.y b.y  x
#> 1   2  20 200   2  40 25
#> 2   2  35 350   5  50 35
#> 3   4  40 400   4  40 45
#> 4  NA  NA 150   1  10 15
#> 5  NA  NA 550   6  60 55
#> 6  NA  NA 270   2  50 65
```


#### full_join() {-}

`full_join()` includes all observations from both `x` and `y`:


```r

# Merging tables by the "a" variable

df1 %>%
  
  full_join(df2, by = "a")
#> Warning in full_join(., df2, by = "a"): Detected an unexpected many-to-many relationship between
#> `x` and `y`.
#> ℹ Row 2 of `x` matches multiple rows in `y`.
#> ℹ Row 2 of `y` matches multiple rows in `x`.
#> ℹ If a many-to-many relationship is expected, set
#>   `relationship = "many-to-many"` to silence this warning.
#>   a b.x   c b.y  x   z
#> 1 1  10 100  10 15 150
#> 2 2  20 200  40 25 200
#> 3 2  20 200  50 65 270
#> 4 3  30 300  NA NA  NA
#> 5 2  35 350  40 25 200
#> 6 2  35 350  50 65 270
#> 7 4  40 400  40 45 400
#> 8 5  NA  NA  50 35 350
#> 9 6  NA  NA  60 55 550
```



```r

# Merging tables by the "a" and "b" variable

df1 %>%
  
  full_join(df2, by = c("a", "b"))
#>   a  b   c  x   z
#> 1 1 10 100 15 150
#> 2 2 20 200 NA  NA
#> 3 3 30 300 NA  NA
#> 4 2 35 350 NA  NA
#> 5 4 40 400 45 400
#> 6 2 40  NA 25 200
#> 7 5 50  NA 35 350
#> 8 6 60  NA 55 550
#> 9 2 50  NA 65 270
```



```r

## Merging tables by the "c" and "z" variable (Have different variable names)

df1 %>%
  
  full_join(df2, by = c("c" = "z"))
#>   a.x b.x   c a.y b.y  x
#> 1   1  10 100  NA  NA NA
#> 2   2  20 200   2  40 25
#> 3   3  30 300  NA  NA NA
#> 4   2  35 350   5  50 35
#> 5   4  40 400   4  40 45
#> 6  NA  NA 150   1  10 15
#> 7  NA  NA 550   6  60 55
#> 8  NA  NA 270   2  50 65
```


### Filtering joins {-}

Filtering joins match observations in the same way as mutating joins, but affect the observations, not the variables. There are two types:

* `semi_join(x, y)` - **keeps** all observations in `x` that have a match in `y`
* `anti_join(x, y)` - **drops** all observations in `x` that have a match in `y`



```r

df1 %>%
  
  semi_join(df2, by = "a")
#>   a  b   c
#> 1 1 10 100
#> 2 2 20 200
#> 3 2 35 350
#> 4 4 40 400
```



```r

df1 %>%
  
  semi_join(df2, by = c("a", "b"))
#>   a  b   c
#> 1 1 10 100
#> 2 4 40 400
```



```r

df1 %>%
  
  anti_join(df2, by = "a")
#>   a  b   c
#> 1 3 30 300
```



```r

df1 %>%
  
  anti_join(df2, by = c("a", "b"))
#>   a  b   c
#> 1 2 20 200
#> 2 3 30 300
#> 3 2 35 350
```


### Set Operations {-}

Set operations expect `x` and `y` tables to have the same variables, and treat the observations as sets:

* `intersect(x, y)` - returns only observations in both `x` and `y`
* `union(x, y)` - returns unique observations in both `x` and `y`
* `setdiff(x, y)` - returns observations in `x`, but not in `y`


We will first create toy data frames and then apply these functions to them:



```r

df1 <- data.frame(
  
  a = c(1, 2, 3, 4, 5),
  
  b = c(10, 20, 30, 40, 50)
  
)

df1
#>   a  b
#> 1 1 10
#> 2 2 20
#> 3 3 30
#> 4 4 40
#> 5 5 50
```


```r

df2 <- data.frame(
  
  a = c(1, 2, 3, 4, 5),
  
  b = c(10, 15, 30, 45, 65)
  
)

df2
#>   a  b
#> 1 1 10
#> 2 2 15
#> 3 3 30
#> 4 4 45
#> 5 5 65
```


```r

intersect(df1, df2)
#>   a  b
#> 1 1 10
#> 2 3 30
```


```r

union(df1, df2)
#>   a  b
#> 1 1 10
#> 2 2 20
#> 3 3 30
#> 4 4 40
#> 5 5 50
#> 6 2 15
#> 7 4 45
#> 8 5 65
```


```r

setdiff(df1, df2)
#>   a  b
#> 1 2 20
#> 2 4 40
#> 3 5 50
```


```r

setdiff(df2, df1)
#>   a  b
#> 1 2 15
#> 2 4 45
#> 3 5 65
```



### Practice Data Sets {-}


Here are two data sets that you can use to practice two-table verbs:



```r

data1 <- data.frame(
  
  Name = c("James", "Linda", "Jim", "Margo", "Nick", "Stacy", "Mary", "Tom", "Anna", "Bob", "Jeniffer", "Lucas", "Amy"),
  
  Age = c(22, 56, 34, 48, 19, 25, 31, 68, 72, 42, 39, 52, 39),
  
  State = c("California", "New York", "New York", "California", "Michigan", "Texas", "Ohio", "Arizona", "Texas", "Florida", "Nebraska", "Indiana", "Florida"),
  
  state_abr = c("CA", "NY", "NY", "CA", "MI", "TX", "OH", "AZ", "TX", "FL", "NE", "IN", "FL"),
  
  City = c("Los Angeles", "New York", "Buffalo", "San Diego", "Detroit", "Austin", "Cleveland", "Phoenix", "Houston", "Tampa", "Lincoln", "Indianapolis", "Miami"),
  
  Salary = c(30000, 96500, 72000, 59000, 54300, 25000, 61000, 64000, 74700, 40000, 83000, 92400, 82000)
  
)

data1
#>        Name Age      State state_abr         City Salary
#> 1     James  22 California        CA  Los Angeles  30000
#> 2     Linda  56   New York        NY     New York  96500
#> 3       Jim  34   New York        NY      Buffalo  72000
#> 4     Margo  48 California        CA    San Diego  59000
#> 5      Nick  19   Michigan        MI      Detroit  54300
#> 6     Stacy  25      Texas        TX       Austin  25000
#> 7      Mary  31       Ohio        OH    Cleveland  61000
#> 8       Tom  68    Arizona        AZ      Phoenix  64000
#> 9      Anna  72      Texas        TX      Houston  74700
#> 10      Bob  42    Florida        FL        Tampa  40000
#> 11 Jeniffer  39   Nebraska        NE      Lincoln  83000
#> 12    Lucas  52    Indiana        IN Indianapolis  92400
#> 13      Amy  39    Florida        FL        Miami  82000
```



```r

data2 <- data.frame(
  
  State = c("Washington", "Florida", "Nebraska", "Indiana", "Florida","California", "New York", "New York", "California", "Michigan", "Texas", "Ohio", "Arizona", "Utah"),
  
  state_abbriviation = c("WA", "FL", "NE", "IN", "FL","CA", "NY", "NY", "CA", "MI", "TX", "OH", "AZ", "UT"),
  
  City = c( "Seatle", "Tampa", "Lincoln", "Indianapolis", "Miami","Los Angeles", "New York", "Ithaca", "San Francisco", "Detroit", "Dallas", "Cleveland", "Phoenix", "Salt Lake City"),
  
  Average_salary = c(63500, 53900, 59900, 59800, 57900, 79000, 80000, 75000, 85000, 54000, 63800, 57000, 61000, 58600)
  
) 

data2
#>         State state_abbriviation           City
#> 1  Washington                 WA         Seatle
#> 2     Florida                 FL          Tampa
#> 3    Nebraska                 NE        Lincoln
#> 4     Indiana                 IN   Indianapolis
#> 5     Florida                 FL          Miami
#> 6  California                 CA    Los Angeles
#> 7    New York                 NY       New York
#> 8    New York                 NY         Ithaca
#> 9  California                 CA  San Francisco
#> 10   Michigan                 MI        Detroit
#> 11      Texas                 TX         Dallas
#> 12       Ohio                 OH      Cleveland
#> 13    Arizona                 AZ        Phoenix
#> 14       Utah                 UT Salt Lake City
#>    Average_salary
#> 1           63500
#> 2           53900
#> 3           59900
#> 4           59800
#> 5           57900
#> 6           79000
#> 7           80000
#> 8           75000
#> 9           85000
#> 10          54000
#> 11          63800
#> 12          57000
#> 13          61000
#> 14          58600
```




