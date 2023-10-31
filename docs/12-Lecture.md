---
output:
  html_document: default
  pdf_document: default
---
# Lecture 12 {-}

&nbsp;


## tidyr Package {-}



Normally 80% of data analysis is spent on the cleaning and preparing data. And it’s not just a first step, but it must be repeated many times over the course of analysis as new problems come to light or new data is collected. In this lecture we will learn how to organize data in R, an organization called **tidy data**. 


The principles of tidy data provide a standard way to organize data values within a dataset. The tidy data standard has been designed to facilitate initial exploration and analysis of the data, and to simplify the development of data analysis tools that work well together. Tidy datasets provide a standardized way to link the structure of a dataset (its physical layout) with its semantics (its meaning).


Most statistical datasets are data frames made up of **rows** and **columns**. A dataset is a collection of values, usually either numbers (if quantitative) or strings (if qualitative). Values are organised in two ways. Every value belongs to a **variable** and an **observation**. A variable contains all values that measure the same underlying attribute across units. An observation contains all values measured on the same unit across attributes. For instance, consider the following datasets:



```r

print(table1)
#> # A tibble: 6 × 4
#>   country      year  cases population
#>   <chr>       <dbl>  <dbl>      <dbl>
#> 1 Afghanistan  1999    745   19987071
#> 2 Afghanistan  2000   2666   20595360
#> 3 Brazil       1999  37737  172006362
#> 4 Brazil       2000  80488  174504898
#> 5 China        1999 212258 1272915272
#> 6 China        2000 213766 1280428583
```


```r

print(table2)
#> # A tibble: 12 × 4
#>    country      year type            count
#>    <chr>       <dbl> <chr>           <dbl>
#>  1 Afghanistan  1999 cases             745
#>  2 Afghanistan  1999 population   19987071
#>  3 Afghanistan  2000 cases            2666
#>  4 Afghanistan  2000 population   20595360
#>  5 Brazil       1999 cases           37737
#>  6 Brazil       1999 population  172006362
#>  7 Brazil       2000 cases           80488
#>  8 Brazil       2000 population  174504898
#>  9 China        1999 cases          212258
#> 10 China        1999 population 1272915272
#> 11 China        2000 cases          213766
#> 12 China        2000 population 1280428583
```


```r

print(table3)
#> # A tibble: 6 × 3
#>   country      year rate             
#>   <chr>       <dbl> <chr>            
#> 1 Afghanistan  1999 745/19987071     
#> 2 Afghanistan  2000 2666/20595360    
#> 3 Brazil       1999 37737/172006362  
#> 4 Brazil       2000 80488/174504898  
#> 5 China        1999 212258/1272915272
#> 6 China        2000 213766/1280428583
```


```r

print(table4a) # cases
#> # A tibble: 3 × 3
#>   country     `1999` `2000`
#>   <chr>        <dbl>  <dbl>
#> 1 Afghanistan    745   2666
#> 2 Brazil       37737  80488
#> 3 China       212258 213766
```


```r

print(table4b) # population
#> # A tibble: 3 × 3
#>   country         `1999`     `2000`
#>   <chr>            <dbl>      <dbl>
#> 1 Afghanistan   19987071   20595360
#> 2 Brazil       172006362  174504898
#> 3 China       1272915272 1280428583
```


They all show the same data organised in four different ways. Each dataset shows the same values of four variables _country_, _year_, _population_, and _cases_, but each dataset organises the values in a different way. Even though these are all representations of the same underlying data, they are not equally easy to use. One dataset, the tidy dataset, will be much easier to work with inside the tidyverse.


There are three interrelated rules which make a dataset tidy:

* Each variable must have its own column
* Each observation must have its own row
* Each value must have its own cell

There are two main advantages of working with tidy data:

1. There’s a general advantage to picking one consistent way of storing data. If you have a consistent data structure, it’s easier to learn the tools that work with it because they have an underlying uniformity.

2. There’s a specific advantage to placing variables in columns because it allows R’s vectorised nature to shine.


### Pivoting {-}

#### pivot_longer() function {-}

Unfortunately, most data that you will encounter will be untidy. For example, consider `table4a`:



```r

print(table4a) # cases
#> # A tibble: 3 × 3
#>   country     `1999` `2000`
#>   <chr>        <dbl>  <dbl>
#> 1 Afghanistan    745   2666
#> 2 Brazil       37737  80488
#> 3 China       212258 213766
```


In this dataset some of the column names are not names of variables, but _values_ of a variable. Specifically, the column names `1999` and `2000` represent values of the year variable, the values in the `1999` and `2000` columns represent values of the cases variable, and each row represents two observations, not one.


To tidy a dataset like this, we need to **pivot** the offending columns into a new pair of variables. To describe that operation we need three parameters:

* The set of columns whose names are values, not variables. In this example, those are the columns `1999` and `2000`
* The name of the variable to move the column names to. Here it is `year`
* The name of the variable to move the column values to. Here it’s `cases`

Together those parameters generate the call to `pivot_longer()` function:


```r

table4a %>% 
  
  pivot_longer(c(`1999`, `2000`), names_to = "year", values_to = "cases")
#> # A tibble: 6 × 3
#>   country     year   cases
#>   <chr>       <chr>  <dbl>
#> 1 Afghanistan 1999     745
#> 2 Afghanistan 2000    2666
#> 3 Brazil      1999   37737
#> 4 Brazil      2000   80488
#> 5 China       1999  212258
#> 6 China       2000  213766
```


Let's repeat the same procedure with `table4b`:



```r

table4b %>%
  
  pivot_longer(c(`1999`, `2000`), names_to = "year", values_to = "population")
#> # A tibble: 6 × 3
#>   country     year  population
#>   <chr>       <chr>      <dbl>
#> 1 Afghanistan 1999    19987071
#> 2 Afghanistan 2000    20595360
#> 3 Brazil      1999   172006362
#> 4 Brazil      2000   174504898
#> 5 China       1999  1272915272
#> 6 China       2000  1280428583
```

Now let's put them together and create a complete dataset:


```r

tidy4a <- table4a %>% 
  
  pivot_longer(c(`1999`, `2000`), names_to = "year", values_to = "cases")



tidy4b <- table4b %>% 
  
  pivot_longer(c(`1999`, `2000`), names_to = "year", values_to = "population")


tidy_4a_4b <- left_join(tidy4a, tidy4b)
#> Joining, by = c("country", "year")

print(tidy_4a_4b)
#> # A tibble: 6 × 4
#>   country     year   cases population
#>   <chr>       <chr>  <dbl>      <dbl>
#> 1 Afghanistan 1999     745   19987071
#> 2 Afghanistan 2000    2666   20595360
#> 3 Brazil      1999   37737  172006362
#> 4 Brazil      2000   80488  174504898
#> 5 China       1999  212258 1272915272
#> 6 China       2000  213766 1280428583
```



#### pivot_wider() function {-}

`pivot_wider()` is the opposite of `pivot_longer()`. You use it when an observation is scattered across multiple rows. For instance, in `table2` an observation is a country in a year, but each observation is spread across two rows:


```r

print(table2)
#> # A tibble: 12 × 4
#>    country      year type            count
#>    <chr>       <dbl> <chr>           <dbl>
#>  1 Afghanistan  1999 cases             745
#>  2 Afghanistan  1999 population   19987071
#>  3 Afghanistan  2000 cases            2666
#>  4 Afghanistan  2000 population   20595360
#>  5 Brazil       1999 cases           37737
#>  6 Brazil       1999 population  172006362
#>  7 Brazil       2000 cases           80488
#>  8 Brazil       2000 population  174504898
#>  9 China        1999 cases          212258
#> 10 China        1999 population 1272915272
#> 11 China        2000 cases          213766
#> 12 China        2000 population 1280428583
```


To tidy this up, we only need two parameters:

* The column to take variable names from. Here, it’s `type`
* The column to take values from. Here it’s `count`



```r

table2 %>%
  
  pivot_wider(names_from = type, values_from = count)
#> # A tibble: 6 × 4
#>   country      year  cases population
#>   <chr>       <dbl>  <dbl>      <dbl>
#> 1 Afghanistan  1999    745   19987071
#> 2 Afghanistan  2000   2666   20595360
#> 3 Brazil       1999  37737  172006362
#> 4 Brazil       2000  80488  174504898
#> 5 China        1999 212258 1272915272
#> 6 China        2000 213766 1280428583
```



### Separating and Uniting {-}

#### separate() Function {-}

`separate()` pulls apart one column into multiple columns, by splitting wherever a separator character appears. For example, in `table3` the `rate` column contains both `cases` and `population` variables, and we need to split it into two variables:


```r

print(table3)
#> # A tibble: 6 × 3
#>   country      year rate             
#>   <chr>       <dbl> <chr>            
#> 1 Afghanistan  1999 745/19987071     
#> 2 Afghanistan  2000 2666/20595360    
#> 3 Brazil       1999 37737/172006362  
#> 4 Brazil       2000 80488/174504898  
#> 5 China        1999 212258/1272915272
#> 6 China        2000 213766/1280428583
```

`separate()` takes the name of the column to separate, and the names of the columns to separate into as well as the specific character to separate a column:


```r

table3 %>%
  
  separate(rate, into = c("cases", "population"), sep = "/")
#> # A tibble: 6 × 4
#>   country      year cases  population
#>   <chr>       <dbl> <chr>  <chr>     
#> 1 Afghanistan  1999 745    19987071  
#> 2 Afghanistan  2000 2666   20595360  
#> 3 Brazil       1999 37737  172006362 
#> 4 Brazil       2000 80488  174504898 
#> 5 China        1999 212258 1272915272
#> 6 China        2000 213766 1280428583
```


`separate()` leaves the type of the column as is. However, we can ask it to try and convert to better types using `convert = TRUE`:


```r

table3 %>% 
  
  separate(rate, into = c("cases", "population"), convert = TRUE)
#> # A tibble: 6 × 4
#>   country      year  cases population
#>   <chr>       <dbl>  <int>      <int>
#> 1 Afghanistan  1999    745   19987071
#> 2 Afghanistan  2000   2666   20595360
#> 3 Brazil       1999  37737  172006362
#> 4 Brazil       2000  80488  174504898
#> 5 China        1999 212258 1272915272
#> 6 China        2000 213766 1280428583
```


You can also pass a vector of integers to `sep`. `separate()` will interpret the integers as positions to split at. Positive values start at 1 on the far-left of the strings; negative value start at -1 on the far-right of the strings. Here are some examples:


```r

table3 %>% 
  
  separate(year, into = c("century", "year"), sep = 2)
#> # A tibble: 6 × 4
#>   country     century year  rate             
#>   <chr>       <chr>   <chr> <chr>            
#> 1 Afghanistan 19      99    745/19987071     
#> 2 Afghanistan 20      00    2666/20595360    
#> 3 Brazil      19      99    37737/172006362  
#> 4 Brazil      20      00    80488/174504898  
#> 5 China       19      99    212258/1272915272
#> 6 China       20      00    213766/1280428583
```


```r

table3 %>% 
  
  separate(year, into = c("century", "year"), sep = 3)
#> # A tibble: 6 × 4
#>   country     century year  rate             
#>   <chr>       <chr>   <chr> <chr>            
#> 1 Afghanistan 199     9     745/19987071     
#> 2 Afghanistan 200     0     2666/20595360    
#> 3 Brazil      199     9     37737/172006362  
#> 4 Brazil      200     0     80488/174504898  
#> 5 China       199     9     212258/1272915272
#> 6 China       200     0     213766/1280428583
```


```r

table3 %>% 
  
  separate(year, into = c("century", "year"), sep = -3)
#> # A tibble: 6 × 4
#>   country     century year  rate             
#>   <chr>       <chr>   <chr> <chr>            
#> 1 Afghanistan 1       999   745/19987071     
#> 2 Afghanistan 2       000   2666/20595360    
#> 3 Brazil      1       999   37737/172006362  
#> 4 Brazil      2       000   80488/174504898  
#> 5 China       1       999   212258/1272915272
#> 6 China       2       000   213766/1280428583
```


#### unite() Function {-}

`unite()` is the inverse of `separate()`: it combines multiple columns into a single column. We can use `unite()` to rejoin the _century_ and _year_ columns in the `table5`:


```r

print(table5)
#> # A tibble: 6 × 4
#>   country     century year  rate             
#>   <chr>       <chr>   <chr> <chr>            
#> 1 Afghanistan 19      99    745/19987071     
#> 2 Afghanistan 20      00    2666/20595360    
#> 3 Brazil      19      99    37737/172006362  
#> 4 Brazil      20      00    80488/174504898  
#> 5 China       19      99    212258/1272915272
#> 6 China       20      00    213766/1280428583
```


`unite()` takes a data frame, the name of the new variable to create, and a set of columns to combine:


```r

table5 %>% 
  
  unite(new, century, year)
#> # A tibble: 6 × 3
#>   country     new   rate             
#>   <chr>       <chr> <chr>            
#> 1 Afghanistan 19_99 745/19987071     
#> 2 Afghanistan 20_00 2666/20595360    
#> 3 Brazil      19_99 37737/172006362  
#> 4 Brazil      20_00 80488/174504898  
#> 5 China       19_99 212258/1272915272
#> 6 China       20_00 213766/1280428583
```


The default will place an underscore `_` between the values from different columns. Use `sep` argument to specify how the columns should be merged:



```r

table5 %>% 
  
  unite(new, century, year, sep = "")
#> # A tibble: 6 × 3
#>   country     new   rate             
#>   <chr>       <chr> <chr>            
#> 1 Afghanistan 1999  745/19987071     
#> 2 Afghanistan 2000  2666/20595360    
#> 3 Brazil      1999  37737/172006362  
#> 4 Brazil      2000  80488/174504898  
#> 5 China       1999  212258/1272915272
#> 6 China       2000  213766/1280428583
```


### Missing Values {-}

A value can be missing in one of two possible ways:

* **Explicitly** - flagged with `NA`
* **Implicitly** - simply not present in the data

Consider the following dataset:


```r

stocks <- data.frame(
  
  year   = c(2015, 2015, 2015, 2015, 2016, 2016, 2016),
  
  qtr    = c(   1,    2,    3,    4,    2,    3,    4),
  
  return = c(1.88, 0.59, 0.35,   NA, 0.92, 0.17, 2.66)
)

print(stocks)
#>   year qtr return
#> 1 2015   1   1.88
#> 2 2015   2   0.59
#> 3 2015   3   0.35
#> 4 2015   4     NA
#> 5 2016   2   0.92
#> 6 2016   3   0.17
#> 7 2016   4   2.66
```

There are two missing values in this dataset:

* The return for the fourth quarter of 2015 is explicitly missing, because the cell where its value should be instead contains `NA`
* The return for the first quarter of 2016 is implicitly missing, because it simply does not appear in the dataset

We can make  implicit missing values explicit. One way of doing it is using `pivot_wider()` function:


```r

stocks %>% 
  
  pivot_wider(names_from = year, values_from = return)
#> # A tibble: 4 × 3
#>     qtr `2015` `2016`
#>   <dbl>  <dbl>  <dbl>
#> 1     1   1.88  NA   
#> 2     2   0.59   0.92
#> 3     3   0.35   0.17
#> 4     4  NA      2.66
```

Or we can simply use `complete()` function. It takes a set of columns, and finds all unique combinations. It then ensures the original dataset contains all those values, filling in explicit `NA`s where necessary:


```r

stocks %>% 
  
  complete(year, qtr)
#> # A tibble: 8 × 3
#>    year   qtr return
#>   <dbl> <dbl>  <dbl>
#> 1  2015     1   1.88
#> 2  2015     2   0.59
#> 3  2015     3   0.35
#> 4  2015     4  NA   
#> 5  2016     1  NA   
#> 6  2016     2   0.92
#> 7  2016     3   0.17
#> 8  2016     4   2.66
```

You can fill in these missing values with `fill()`. It takes a set of columns where you want missing values to be replaced by the most recent non-missing value (sometimes called last observation carried forward):


```r

stocks %>% 
  
  complete(year, qtr) %>%
  
  fill(return)
#> # A tibble: 8 × 3
#>    year   qtr return
#>   <dbl> <dbl>  <dbl>
#> 1  2015     1   1.88
#> 2  2015     2   0.59
#> 3  2015     3   0.35
#> 4  2015     4   0.35
#> 5  2016     1   0.35
#> 6  2016     2   0.92
#> 7  2016     3   0.17
#> 8  2016     4   2.66
```


You can change the direction in which to fill in missing values. For instance, let's fill in the missing values with the value that comes right after these missing values:


```r

stocks %>% 
  
  complete(year, qtr) %>%
  
  fill(return, .direction = "up")
#> # A tibble: 8 × 3
#>    year   qtr return
#>   <dbl> <dbl>  <dbl>
#> 1  2015     1   1.88
#> 2  2015     2   0.59
#> 3  2015     3   0.35
#> 4  2015     4   0.92
#> 5  2016     1   0.92
#> 6  2016     2   0.92
#> 7  2016     3   0.17
#> 8  2016     4   2.66
```


`replace_na()` function allows to replace missing values with specific values:



```r

stocks %>% 
  
  complete(year, qtr) %>%
  
  replace_na(list(return = 0))
#> # A tibble: 8 × 3
#>    year   qtr return
#>   <dbl> <dbl>  <dbl>
#> 1  2015     1   1.88
#> 2  2015     2   0.59
#> 3  2015     3   0.35
#> 4  2015     4   0   
#> 5  2016     1   0   
#> 6  2016     2   0.92
#> 7  2016     3   0.17
#> 8  2016     4   2.66
```


Finally, you can drop missing values from your dataset:


```r

stocks %>% 
  
  complete(year, qtr) %>%
  
  drop_na()
#> # A tibble: 6 × 3
#>    year   qtr return
#>   <dbl> <dbl>  <dbl>
#> 1  2015     1   1.88
#> 2  2015     2   0.59
#> 3  2015     3   0.35
#> 4  2016     2   0.92
#> 5  2016     3   0.17
#> 6  2016     4   2.66
```


### Let's Practice {-}

Here are some datasets to practice tidyr functions with. In addition, below you can find some examples of how these function can be utilized. Try to understand what these function are doing. The datasets are available on Courseworks (`Lecture12_data` file).


#### Relationship between Income and Religion in the US {-}


```r

relig_income %>% 
  
  pivot_longer(-religion, names_to = "income", values_to = "frequency")
#> # A tibble: 180 × 3
#>    religion income             frequency
#>    <chr>    <chr>                  <dbl>
#>  1 Agnostic <$10k                     27
#>  2 Agnostic $10-20k                   34
#>  3 Agnostic $20-30k                   60
#>  4 Agnostic $30-40k                   81
#>  5 Agnostic $40-50k                   76
#>  6 Agnostic $50-75k                  137
#>  7 Agnostic $75-100k                 122
#>  8 Agnostic $100-150k                109
#>  9 Agnostic >150k                     84
#> 10 Agnostic Don't know/refused        96
#> # … with 170 more rows
```


#### Tuberculosis data from World Health Organization (WHO) {-}


```r

who1 <- who %>%
  
    pivot_longer(
    
    cols = new_sp_m014:newrel_f65,
    
    names_to = "key",
    
    values_to = "cases", 
    
    values_drop_na = TRUE)

print(who1)
#> # A tibble: 76,046 × 6
#>    country     iso2  iso3   year key          cases
#>    <chr>       <chr> <chr> <dbl> <chr>        <dbl>
#>  1 Afghanistan AF    AFG    1997 new_sp_m014      0
#>  2 Afghanistan AF    AFG    1997 new_sp_m1524    10
#>  3 Afghanistan AF    AFG    1997 new_sp_m2534     6
#>  4 Afghanistan AF    AFG    1997 new_sp_m3544     3
#>  5 Afghanistan AF    AFG    1997 new_sp_m4554     5
#>  6 Afghanistan AF    AFG    1997 new_sp_m5564     2
#>  7 Afghanistan AF    AFG    1997 new_sp_m65       0
#>  8 Afghanistan AF    AFG    1997 new_sp_f014      5
#>  9 Afghanistan AF    AFG    1997 new_sp_f1524    38
#> 10 Afghanistan AF    AFG    1997 new_sp_f2534    36
#> # … with 76,036 more rows
```



```r

## just run this code

who2 <- who1 %>% 
  
  mutate(key = stringr::str_replace(key, "newrel", "new_rel"))
```



```r

who3 <- who2 %>% 
  
  separate(key, c("new", "type", "sexage"), sep = "_")

print(who3)
#> # A tibble: 76,046 × 8
#>    country     iso2  iso3   year new   type  sexage cases
#>    <chr>       <chr> <chr> <dbl> <chr> <chr> <chr>  <dbl>
#>  1 Afghanistan AF    AFG    1997 new   sp    m014       0
#>  2 Afghanistan AF    AFG    1997 new   sp    m1524     10
#>  3 Afghanistan AF    AFG    1997 new   sp    m2534      6
#>  4 Afghanistan AF    AFG    1997 new   sp    m3544      3
#>  5 Afghanistan AF    AFG    1997 new   sp    m4554      5
#>  6 Afghanistan AF    AFG    1997 new   sp    m5564      2
#>  7 Afghanistan AF    AFG    1997 new   sp    m65        0
#>  8 Afghanistan AF    AFG    1997 new   sp    f014       5
#>  9 Afghanistan AF    AFG    1997 new   sp    f1524     38
#> 10 Afghanistan AF    AFG    1997 new   sp    f2534     36
#> # … with 76,036 more rows
```


```r

who4 <- who3 %>%
  
  separate(sexage, c("sex", "age"), sep = 1)

print(who4)
#> # A tibble: 76,046 × 9
#>    country   iso2  iso3   year new   type  sex   age   cases
#>    <chr>     <chr> <chr> <dbl> <chr> <chr> <chr> <chr> <dbl>
#>  1 Afghanis… AF    AFG    1997 new   sp    m     014       0
#>  2 Afghanis… AF    AFG    1997 new   sp    m     1524     10
#>  3 Afghanis… AF    AFG    1997 new   sp    m     2534      6
#>  4 Afghanis… AF    AFG    1997 new   sp    m     3544      3
#>  5 Afghanis… AF    AFG    1997 new   sp    m     4554      5
#>  6 Afghanis… AF    AFG    1997 new   sp    m     5564      2
#>  7 Afghanis… AF    AFG    1997 new   sp    m     65        0
#>  8 Afghanis… AF    AFG    1997 new   sp    f     014       5
#>  9 Afghanis… AF    AFG    1997 new   sp    f     1524     38
#> 10 Afghanis… AF    AFG    1997 new   sp    f     2534     36
#> # … with 76,036 more rows
```


#### Billboard charts data {-}


```r

billboard2 <- billboard %>% 
  
  pivot_longer(
    
    cols = starts_with("wk"), 
    
    names_to = "week",
    
    values_to = "rank", 
    
    values_drop_na = TRUE)

print(billboard2)
#> # A tibble: 5,307 × 5
#>    artist  track                   date.entered week   rank
#>    <chr>   <chr>                   <date>       <chr> <dbl>
#>  1 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk1      87
#>  2 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk2      82
#>  3 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk3      72
#>  4 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk4      77
#>  5 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk5      87
#>  6 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk6      94
#>  7 2 Pac   Baby Don't Cry (Keep... 2000-02-26   wk7      99
#>  8 2Ge+her The Hardest Part Of ... 2000-09-02   wk1      91
#>  9 2Ge+her The Hardest Part Of ... 2000-09-02   wk2      87
#> 10 2Ge+her The Hardest Part Of ... 2000-09-02   wk3      92
#> # … with 5,297 more rows
```



```r

billboard3 <- billboard %>% 
  
  pivot_longer(
    
    cols = starts_with("wk"), 
    
    names_to = "week",
    
    names_prefix = "wk",
    
    names_transform = as.integer,
    
    values_to = "rank", 
    
    values_drop_na = TRUE)


print(billboard3)
#> # A tibble: 5,307 × 5
#>    artist  track                   date.entered  week  rank
#>    <chr>   <chr>                   <date>       <int> <dbl>
#>  1 2 Pac   Baby Don't Cry (Keep... 2000-02-26       1    87
#>  2 2 Pac   Baby Don't Cry (Keep... 2000-02-26       2    82
#>  3 2 Pac   Baby Don't Cry (Keep... 2000-02-26       3    72
#>  4 2 Pac   Baby Don't Cry (Keep... 2000-02-26       4    77
#>  5 2 Pac   Baby Don't Cry (Keep... 2000-02-26       5    87
#>  6 2 Pac   Baby Don't Cry (Keep... 2000-02-26       6    94
#>  7 2 Pac   Baby Don't Cry (Keep... 2000-02-26       7    99
#>  8 2Ge+her The Hardest Part Of ... 2000-09-02       1    91
#>  9 2Ge+her The Hardest Part Of ... 2000-09-02       2    87
#> 10 2Ge+her The Hardest Part Of ... 2000-09-02       3    92
#> # … with 5,297 more rows
```



```r

billboard3 %>%
  
  arrange(artist, track, week)
#> # A tibble: 5,307 × 5
#>    artist  track                   date.entered  week  rank
#>    <chr>   <chr>                   <date>       <int> <dbl>
#>  1 2 Pac   Baby Don't Cry (Keep... 2000-02-26       1    87
#>  2 2 Pac   Baby Don't Cry (Keep... 2000-02-26       2    82
#>  3 2 Pac   Baby Don't Cry (Keep... 2000-02-26       3    72
#>  4 2 Pac   Baby Don't Cry (Keep... 2000-02-26       4    77
#>  5 2 Pac   Baby Don't Cry (Keep... 2000-02-26       5    87
#>  6 2 Pac   Baby Don't Cry (Keep... 2000-02-26       6    94
#>  7 2 Pac   Baby Don't Cry (Keep... 2000-02-26       7    99
#>  8 2Ge+her The Hardest Part Of ... 2000-09-02       1    91
#>  9 2Ge+her The Hardest Part Of ... 2000-09-02       2    87
#> 10 2Ge+her The Hardest Part Of ... 2000-09-02       3    92
#> # … with 5,297 more rows
```
