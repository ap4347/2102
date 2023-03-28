# Lecture 10 {-}

&nbsp;


## Tidyverse Family of Packages {-}

The **_data frame_** is a key data structure in statistics and in R. The basic structure of a data frame is that there is one observation per row and each column represents a variable, a measure, feature, or characteristic of that observation. Before you can conduct any analyses or draw any conclusions, you often need to reorganize your data. The **Tidyverse** is a collection of R packages (developed by RStudio’s chief scientist Hadley Wickham) that provides an efficient, fast, and well-documented workflow for general data modeling, wrangling, and visualization tasks.


The Tidyverse introduces a set of useful data analysis packages to help streamline your work in R. In particular, the Tidyverse was designed to address the top three common issues that arise when dealing with data analysis in  base R: (1) Results obtained from a base R function often depend on the type of data being used; (2) When R expressions are used in a non-standard way, they can confuse beginners; (3) Hidden arguments often have various default operations that beginners are unaware of.


The core Tidyverse includes the packages that you’re likely to use in everyday data analyses. 

* `ggplot2` - ggplot2 is a system for declaratively creating graphics, based on The Grammar of Graphics. You provide the data, tell ggplot2 how to map variables to aesthetics, what graphical primitives to use, and it takes care of the details.

* `dplyr` - dplyr provides a grammar of data manipulation, providing a consistent set of verbs that solve the most common data manipulation challenges.

* `tidyr` - tidyr provides a set of functions that help you get to tidy data. Tidy data is data with a consistent form: in brief, every variable goes in a column, and every column is a variable.

* `readr` - readr provides a fast and friendly way to read rectangular data (like csv, tsv, and fwf). It is designed to flexibly parse many types of data found in the wild, while still cleanly failing when data unexpectedly changes.

* `purrr` - purrr enhances R’s functional programming (FP) toolkit by providing a complete and consistent set of tools for working with functions and vectors. Once you master the basic concepts, purrr allows you to replace many for loops with code that is easier to write and more expressive. 

* `tibble` - tibble is a modern re-imagining of the data frame, keeping what time has proven to be effective, and throwing out what it has not. Tibbles are data.frames that are lazy and surly: they do less and complain more forcing you to confront problems earlier, typically leading to cleaner, more expressive code. 

* `stringr` - stringr provides a cohesive set of functions designed to make working with strings as easy as possible. It is built on top of stringi, which uses the ICU C library to provide fast, correct implementations of common string manipulations.

* `forcats` - forcats provides a suite of useful tools that solve common problems with factors. R uses factors to handle categorical variables, variables that have a fixed and known set of possible values.


The Tidyverse also includes many other packages with more specialized usage. They are not loaded automatically with Tidyverse, so you'll need to load each one with its own call.


To install the Tidyverse packages run the following code in the console:


```r

install.packages("tidyverse")

```


Now the Tidyverse is available in R, but it is not activated yet. Whenever you start a new R session and plan to use the Tidyverse packages, you will need to activate the package by calling the `library(tidyverse)` function in the console:


```r

library(tidyverse)
#> ── Attaching packages ─────────────────── tidyverse 1.3.2 ──
#> ✔ ggplot2 3.3.6      ✔ purrr   0.3.5 
#> ✔ tibble  3.1.8      ✔ dplyr   1.0.10
#> ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
#> ✔ readr   2.1.3      ✔ forcats 0.5.2 
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
```


Today we will start learning the Tidyverse family of packages by introducing the `dplyr` package.


We will be working with a `nyc_flights` data set that provides information about flights departed New York City in 2013 (the data set is available on Courseworks). It contains 336 776 observations (rows) and 19 variables (columns). Let's import this data set into R: 



```r

flights <- read.csv(file = "C:/Users/alexp/OneDrive/Documents/nyc_flights.csv", header = T)
```

Let's convert our data frame into a `tibble` data frame (we will discuss this format in details later in the semester):


```r

flights <- as_tibble(flights)
```


&nbsp;


## dplyr Package {-}

As mentioned earlier, `dplyr` provides a grammar of data manipulation, providing a consistent set of verbs that solve the most common data manipulation challenges such as selecting important variables, filtering out key observations, creating new variables, computing summaries, and so on. 


In this lecture you are going to learn the key dplyr functions that allow you to solve the vast majority of your data manipulation challenges. All of the functions that we will discuss today will have a few common characteristics. In particular,

* The first argument is a data frame

* The subsequent arguments describe what to do with the data frame specified in the first argument, and you can refer to columns in the data frame directly without using the $ operator (just use the column names)

* The return result of a function is a new data frame


dplyr aims to provide a function for each basic verb of data manipulation. These verbs can be organised into three categories based on the component of the data set that they work with:

* Rows:

  * `filter()` - chooses rows based on column values
  * `slice()` - chooses rows based on location
  * `arrange()` -  changes the order of the rows
  
* Columns:

  * `select()` - changes whether or not a column is included
  * `rename()` - changes the name of columns
  * `mutate()` - changes the values of columns and creates new columns
  * `relocate()` - changes the order of the columns
  
* Groups of rows:

  * `group_by()` - changes the scope of each function from operating on the entire data set to operating on it group-by-group
  * `summarize()` - collapses a group into a single row
  


### filter() Function {-}

`filter()` allows you to subset observations based on their values. The first argument is the name of the data frame, the second and subsequent arguments are the expressions that filter the data frame. For instance, let's select all flights on January 1st:



```r

filter(flights, month == 1, day == 1)
#> # A tibble: 842 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013     1     1     517     515       2     830     819
#>  2  2013     1     1     533     529       4     850     830
#>  3  2013     1     1     542     540       2     923     850
#>  4  2013     1     1     544     545      -1    1004    1022
#>  5  2013     1     1     554     600      -6     812     837
#>  6  2013     1     1     554     558      -4     740     728
#>  7  2013     1     1     555     600      -5     913     854
#>  8  2013     1     1     557     600      -3     709     723
#>  9  2013     1     1     557     600      -3     838     846
#> 10  2013     1     1     558     600      -2     753     745
#> # … with 832 more rows, 11 more variables: arr_delay <int>,
#> #   carrier <chr>, flight <int>, tailnum <chr>,
#> #   origin <chr>, dest <chr>, air_time <int>,
#> #   distance <int>, hour <int>, minute <int>,
#> #   time_hour <chr>, and abbreviated variable names
#> #   ¹​dep_time, ²​sched_dep_time, ³​dep_delay, ⁴​arr_time,
#> #   ⁵​sched_arr_time
```

When you run that line of code, dplyr executes the filtering operation and returns a new data frame. dplyr functions never modify their inputs, so if you want to save the result, you’ll need to use the assignment operator, `<-` :


```r

jan1 <- filter(flights, month == 1, day == 1)
```


Let's find all flights that departed in November or December:


```r

filter(flights, month == 11 | month == 12)
#> # A tibble: 55,403 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013    11     1       5    2359       6     352     345
#>  2  2013    11     1      35    2250     105     123    2356
#>  3  2013    11     1     455     500      -5     641     651
#>  4  2013    11     1     539     545      -6     856     827
#>  5  2013    11     1     542     545      -3     831     855
#>  6  2013    11     1     549     600     -11     912     923
#>  7  2013    11     1     550     600     -10     705     659
#>  8  2013    11     1     554     600      -6     659     701
#>  9  2013    11     1     554     600      -6     826     827
#> 10  2013    11     1     554     600      -6     749     751
#> # … with 55,393 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```

We could do the same operation using the `%in%` operator:


```r

filter(flights, month %in% c(11, 12))
#> # A tibble: 55,403 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013    11     1       5    2359       6     352     345
#>  2  2013    11     1      35    2250     105     123    2356
#>  3  2013    11     1     455     500      -5     641     651
#>  4  2013    11     1     539     545      -6     856     827
#>  5  2013    11     1     542     545      -3     831     855
#>  6  2013    11     1     549     600     -11     912     923
#>  7  2013    11     1     550     600     -10     705     659
#>  8  2013    11     1     554     600      -6     659     701
#>  9  2013    11     1     554     600      -6     826     827
#> 10  2013    11     1     554     600      -6     749     751
#> # … with 55,393 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```


### slice() Function {-}

`slice()` function allows you to index rows by their (integer) locations. It can select, remove, and duplicate rows.

For instance, let's get observations from rows 5 through 10:


```r

slice(flights, 5:10)
#> # A tibble: 6 × 19
#>    year month   day dep_time sched…¹ dep_d…² arr_t…³ sched…⁴
#>   <int> <int> <int>    <int>   <int>   <int>   <int>   <int>
#> 1  2013     1     1      554     600      -6     812     837
#> 2  2013     1     1      554     558      -4     740     728
#> 3  2013     1     1      555     600      -5     913     854
#> 4  2013     1     1      557     600      -3     709     723
#> 5  2013     1     1      557     600      -3     838     846
#> 6  2013     1     1      558     600      -2     753     745
#> # … with 11 more variables: arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​sched_dep_time, ²​dep_delay, ³​arr_time,
#> #   ⁴​sched_arr_time
```


Let's select all rows except the first four (this option can be used to drop some observations from a data set):


```r

slice(flights, -(1:4))
#> # A tibble: 336,772 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013     1     1     554     600      -6     812     837
#>  2  2013     1     1     554     558      -4     740     728
#>  3  2013     1     1     555     600      -5     913     854
#>  4  2013     1     1     557     600      -3     709     723
#>  5  2013     1     1     557     600      -3     838     846
#>  6  2013     1     1     558     600      -2     753     745
#>  7  2013     1     1     558     600      -2     849     851
#>  8  2013     1     1     558     600      -2     853     856
#>  9  2013     1     1     558     600      -2     924     917
#> 10  2013     1     1     558     600      -2     923     937
#> # … with 336,762 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```


Similar to `head()` and `tail()` functions, `slice_head()` and `slice_tail()` can be used to display top and bottom rows in the data set, respectively. Let's print first and last 3 rows in the flights data set:


```r

slice_head(flights, n = 3)
#> # A tibble: 3 × 19
#>    year month   day dep_time sched…¹ dep_d…² arr_t…³ sched…⁴
#>   <int> <int> <int>    <int>   <int>   <int>   <int>   <int>
#> 1  2013     1     1      517     515       2     830     819
#> 2  2013     1     1      533     529       4     850     830
#> 3  2013     1     1      542     540       2     923     850
#> # … with 11 more variables: arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​sched_dep_time, ²​dep_delay, ³​arr_time,
#> #   ⁴​sched_arr_time
```


```r

slice_tail(flights, n = 3)
#> # A tibble: 3 × 19
#>    year month   day dep_time sched…¹ dep_d…² arr_t…³ sched…⁴
#>   <int> <int> <int>    <int>   <int>   <int>   <int>   <int>
#> 1  2013     9    30       NA    1210      NA      NA    1330
#> 2  2013     9    30       NA    1159      NA      NA    1344
#> 3  2013     9    30       NA     840      NA      NA    1020
#> # … with 11 more variables: arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​sched_dep_time, ²​dep_delay, ³​arr_time,
#> #   ⁴​sched_arr_time
```

Use the `slice_sample()` function to randomly select rows. Use the option `prop` to choose a certain proportion of the cases:


```r

slice_sample(flights, n = 10)
#> # A tibble: 10 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013    10    15     655     700      -5    1106    1056
#>  2  2013    11     5     727     725       2     913     927
#>  3  2013    10    21    1019    1026      -7    1132    1130
#>  4  2013    11    12     701     705      -4    1028     955
#>  5  2013    10     7    1143    1145      -2    1432    1450
#>  6  2013     6    18     839     841      -2     949    1004
#>  7  2013     8    26     637     638      -1     838     845
#>  8  2013     9    19     650     655      -5     819     824
#>  9  2013     7    30    1707    1710      -3    2048    2035
#> 10  2013     8     8    1512    1452      20      NA    1747
#> # … with 11 more variables: arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```


```r

slice_sample(flights, prop = 0.001)
#> # A tibble: 336 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013     6    20     757     800      -3    1100    1104
#>  2  2013     8    21     809     815      -6    1010    1035
#>  3  2013     5     8    1409    1245      84    1605    1455
#>  4  2013     2     1     856     857      -1    1144    1204
#>  5  2013    10     6    1907    1850      17    2050    2025
#>  6  2013     6    13    1745    1755     -10    2052    2010
#>  7  2013    11    28     859     900      -1    1111    1129
#>  8  2013     4    21     844     850      -6     952    1011
#>  9  2013    12    31    1553    1600      -7    1858    1851
#> 10  2013     2     3     905     825      40    1300    1137
#> # … with 326 more rows, 11 more variables: arr_delay <int>,
#> #   carrier <chr>, flight <int>, tailnum <chr>,
#> #   origin <chr>, dest <chr>, air_time <int>,
#> #   distance <int>, hour <int>, minute <int>,
#> #   time_hour <chr>, and abbreviated variable names
#> #   ¹​dep_time, ²​sched_dep_time, ³​dep_delay, ⁴​arr_time,
#> #   ⁵​sched_arr_time
```

Use `replace = TRUE` to take a sample with replacement. 


### arrange() Function {-}

The `arrange()` function is used to change the order of rows in a data set. It takes a data frame and a set of column names (or more complicated expressions) to order by. If you provide more than one column name, each additional column will be used to break ties in the values of preceding columns:


```r

arrange(flights, year, month, day)
#> # A tibble: 336,776 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013     1     1     517     515       2     830     819
#>  2  2013     1     1     533     529       4     850     830
#>  3  2013     1     1     542     540       2     923     850
#>  4  2013     1     1     544     545      -1    1004    1022
#>  5  2013     1     1     554     600      -6     812     837
#>  6  2013     1     1     554     558      -4     740     728
#>  7  2013     1     1     555     600      -5     913     854
#>  8  2013     1     1     557     600      -3     709     723
#>  9  2013     1     1     557     600      -3     838     846
#> 10  2013     1     1     558     600      -2     753     745
#> # … with 336,766 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```

Use `desc()` to re-order by a column in descending order:


```r

arrange(flights, desc(dep_delay))
#> # A tibble: 336,776 × 19
#>     year month   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013     1     9     641     900    1301    1242    1530
#>  2  2013     6    15    1432    1935    1137    1607    2120
#>  3  2013     1    10    1121    1635    1126    1239    1810
#>  4  2013     9    20    1139    1845    1014    1457    2210
#>  5  2013     7    22     845    1600    1005    1044    1815
#>  6  2013     4    10    1100    1900     960    1342    2211
#>  7  2013     3    17    2321     810     911     135    1020
#>  8  2013     6    27     959    1900     899    1236    2226
#>  9  2013     7    22    2257     759     898     121    1026
#> 10  2013    12     5     756    1700     896    1058    2020
#> # … with 336,766 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```



### select() Function {-}

Often you work with large data sets with many columns but only a few are actually of interest to you. `select()` function allows you to rapidly zoom in on a useful subset. You can select columns by name:


```r

select(flights, year, month, day)
#> # A tibble: 336,776 × 3
#>     year month   day
#>    <int> <int> <int>
#>  1  2013     1     1
#>  2  2013     1     1
#>  3  2013     1     1
#>  4  2013     1     1
#>  5  2013     1     1
#>  6  2013     1     1
#>  7  2013     1     1
#>  8  2013     1     1
#>  9  2013     1     1
#> 10  2013     1     1
#> # … with 336,766 more rows
```

You can select all columns between two variables (inclusive):


```r

select(flights, year:day)
#> # A tibble: 336,776 × 3
#>     year month   day
#>    <int> <int> <int>
#>  1  2013     1     1
#>  2  2013     1     1
#>  3  2013     1     1
#>  4  2013     1     1
#>  5  2013     1     1
#>  6  2013     1     1
#>  7  2013     1     1
#>  8  2013     1     1
#>  9  2013     1     1
#> 10  2013     1     1
#> # … with 336,766 more rows
```

You can select all columns except some:


```r

select(flights, -(year:day))
#> # A tibble: 336,776 × 16
#>    dep_time sched_…¹ dep_d…² arr_t…³ sched…⁴ arr_d…⁵ carrier
#>       <int>    <int>   <int>   <int>   <int>   <int> <chr>  
#>  1      517      515       2     830     819      11 UA     
#>  2      533      529       4     850     830      20 UA     
#>  3      542      540       2     923     850      33 AA     
#>  4      544      545      -1    1004    1022     -18 B6     
#>  5      554      600      -6     812     837     -25 DL     
#>  6      554      558      -4     740     728      12 UA     
#>  7      555      600      -5     913     854      19 B6     
#>  8      557      600      -3     709     723     -14 EV     
#>  9      557      600      -3     838     846      -8 B6     
#> 10      558      600      -2     753     745       8 AA     
#> # … with 336,766 more rows, 9 more variables: flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​sched_dep_time, ²​dep_delay, ³​arr_time,
#> #   ⁴​sched_arr_time, ⁵​arr_delay
```

You can do the same operation with `!` operator:


```r

select(flights, !(year:day))
#> # A tibble: 336,776 × 16
#>    dep_time sched_…¹ dep_d…² arr_t…³ sched…⁴ arr_d…⁵ carrier
#>       <int>    <int>   <int>   <int>   <int>   <int> <chr>  
#>  1      517      515       2     830     819      11 UA     
#>  2      533      529       4     850     830      20 UA     
#>  3      542      540       2     923     850      33 AA     
#>  4      544      545      -1    1004    1022     -18 B6     
#>  5      554      600      -6     812     837     -25 DL     
#>  6      554      558      -4     740     728      12 UA     
#>  7      555      600      -5     913     854      19 B6     
#>  8      557      600      -3     709     723     -14 EV     
#>  9      557      600      -3     838     846      -8 B6     
#> 10      558      600      -2     753     745       8 AA     
#> # … with 336,766 more rows, 9 more variables: flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​sched_dep_time, ²​dep_delay, ³​arr_time,
#> #   ⁴​sched_arr_time, ⁵​arr_delay
```

You can use column indexes for column selection:


```r

select(flights, c(1, 5, 8))
#> # A tibble: 336,776 × 3
#>     year sched_dep_time sched_arr_time
#>    <int>          <int>          <int>
#>  1  2013            515            819
#>  2  2013            529            830
#>  3  2013            540            850
#>  4  2013            545           1022
#>  5  2013            600            837
#>  6  2013            558            728
#>  7  2013            600            854
#>  8  2013            600            723
#>  9  2013            600            846
#> 10  2013            600            745
#> # … with 336,766 more rows
```

There are a number of helper functions you can use within `select()`. For example, `starts_with()`, `ends_with()`, `matches()` and `contains()`. These let you quickly match larger blocks of variables that meet some criterion.

Let's select all columns that start with "sched":


```r

select(flights, starts_with("sched"))
#> # A tibble: 336,776 × 2
#>    sched_dep_time sched_arr_time
#>             <int>          <int>
#>  1            515            819
#>  2            529            830
#>  3            540            850
#>  4            545           1022
#>  5            600            837
#>  6            558            728
#>  7            600            854
#>  8            600            723
#>  9            600            846
#> 10            600            745
#> # … with 336,766 more rows
```

You can select all columns in the data set that end with "time":


```r

select(flights, ends_with("time"))
#> # A tibble: 336,776 × 5
#>    dep_time sched_dep_time arr_time sched_arr_time air_time
#>       <int>          <int>    <int>          <int>    <int>
#>  1      517            515      830            819      227
#>  2      533            529      850            830      227
#>  3      542            540      923            850      160
#>  4      544            545     1004           1022      183
#>  5      554            600      812            837      116
#>  6      554            558      740            728      150
#>  7      555            600      913            854      158
#>  8      557            600      709            723       53
#>  9      557            600      838            846      140
#> 10      558            600      753            745      138
#> # … with 336,766 more rows
```

Or suppose you want to select all columns in the data set that contain "ar":


```r

select(flights, contains("ar"))
#> # A tibble: 336,776 × 5
#>     year arr_time sched_arr_time arr_delay carrier
#>    <int>    <int>          <int>     <int> <chr>  
#>  1  2013      830            819        11 UA     
#>  2  2013      850            830        20 UA     
#>  3  2013      923            850        33 AA     
#>  4  2013     1004           1022       -18 B6     
#>  5  2013      812            837       -25 DL     
#>  6  2013      740            728        12 UA     
#>  7  2013      913            854        19 B6     
#>  8  2013      709            723       -14 EV     
#>  9  2013      838            846        -8 B6     
#> 10  2013      753            745         8 AA     
#> # … with 336,766 more rows
```

You can even combine these arguments:


```r

select(flights, starts_with("sched") & ends_with("time"))
#> # A tibble: 336,776 × 2
#>    sched_dep_time sched_arr_time
#>             <int>          <int>
#>  1            515            819
#>  2            529            830
#>  3            540            850
#>  4            545           1022
#>  5            600            837
#>  6            558            728
#>  7            600            854
#>  8            600            723
#>  9            600            846
#> 10            600            745
#> # … with 336,766 more rows
```


### rename() Function {-}

Use `rename()` function to rename columns in a data frame. Suppose we want to rename the "year" and "month" variables and make them uppercase:


```r

rename(flights, YEAR = year, MONTH = month)
#> # A tibble: 336,776 × 19
#>     YEAR MONTH   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1  2013     1     1     517     515       2     830     819
#>  2  2013     1     1     533     529       4     850     830
#>  3  2013     1     1     542     540       2     923     850
#>  4  2013     1     1     544     545      -1    1004    1022
#>  5  2013     1     1     554     600      -6     812     837
#>  6  2013     1     1     554     558      -4     740     728
#>  7  2013     1     1     555     600      -5     913     854
#>  8  2013     1     1     557     600      -3     709     723
#>  9  2013     1     1     557     600      -3     838     846
#> 10  2013     1     1     558     600      -2     753     745
#> # … with 336,766 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```


### relocate() Function {-}

`relocate()` function allows to change the positions of columns in a data frame. It has two useful arguments `.before` and `.after` that helps precisely select a location for a variable:


```r

relocate(flights, year, .after = month)
#> # A tibble: 336,776 × 19
#>    month  year   day dep_t…¹ sched…² dep_d…³ arr_t…⁴ sched…⁵
#>    <int> <int> <int>   <int>   <int>   <int>   <int>   <int>
#>  1     1  2013     1     517     515       2     830     819
#>  2     1  2013     1     533     529       4     850     830
#>  3     1  2013     1     542     540       2     923     850
#>  4     1  2013     1     544     545      -1    1004    1022
#>  5     1  2013     1     554     600      -6     812     837
#>  6     1  2013     1     554     558      -4     740     728
#>  7     1  2013     1     555     600      -5     913     854
#>  8     1  2013     1     557     600      -3     709     723
#>  9     1  2013     1     557     600      -3     838     846
#> 10     1  2013     1     558     600      -2     753     745
#> # … with 336,766 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```


```r

relocate(flights, c(year, month), .before = dep_delay)
#> # A tibble: 336,776 × 19
#>      day dep_t…¹ sched…²  year month dep_d…³ arr_t…⁴ sched…⁵
#>    <int>   <int>   <int> <int> <int>   <int>   <int>   <int>
#>  1     1     517     515  2013     1       2     830     819
#>  2     1     533     529  2013     1       4     850     830
#>  3     1     542     540  2013     1       2     923     850
#>  4     1     544     545  2013     1      -1    1004    1022
#>  5     1     554     600  2013     1      -6     812     837
#>  6     1     554     558  2013     1      -4     740     728
#>  7     1     555     600  2013     1      -5     913     854
#>  8     1     557     600  2013     1      -3     709     723
#>  9     1     557     600  2013     1      -3     838     846
#> 10     1     558     600  2013     1      -2     753     745
#> # … with 336,766 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_time, ²​sched_dep_time, ³​dep_delay,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```



```r

relocate(flights, c(year, month), .after = last_col())
#> # A tibble: 336,776 × 19
#>      day dep_time sched_de…¹ dep_d…² arr_t…³ sched…⁴ arr_d…⁵
#>    <int>    <int>      <int>   <int>   <int>   <int>   <int>
#>  1     1      517        515       2     830     819      11
#>  2     1      533        529       4     850     830      20
#>  3     1      542        540       2     923     850      33
#>  4     1      544        545      -1    1004    1022     -18
#>  5     1      554        600      -6     812     837     -25
#>  6     1      554        558      -4     740     728      12
#>  7     1      555        600      -5     913     854      19
#>  8     1      557        600      -3     709     723     -14
#>  9     1      557        600      -3     838     846      -8
#> 10     1      558        600      -2     753     745       8
#> # … with 336,766 more rows, 12 more variables:
#> #   carrier <chr>, flight <int>, tailnum <chr>,
#> #   origin <chr>, dest <chr>, air_time <int>,
#> #   distance <int>, hour <int>, minute <int>,
#> #   time_hour <chr>, year <int>, month <int>, and
#> #   abbreviated variable names ¹​sched_dep_time, ²​dep_delay,
#> #   ³​arr_time, ⁴​sched_arr_time, ⁵​arr_delay
```


```r

relocate(flights, dep_delay, .before = everything())
#> # A tibble: 336,776 × 19
#>    dep_d…¹  year month   day dep_t…² sched…³ arr_t…⁴ sched…⁵
#>      <int> <int> <int> <int>   <int>   <int>   <int>   <int>
#>  1       2  2013     1     1     517     515     830     819
#>  2       4  2013     1     1     533     529     850     830
#>  3       2  2013     1     1     542     540     923     850
#>  4      -1  2013     1     1     544     545    1004    1022
#>  5      -6  2013     1     1     554     600     812     837
#>  6      -4  2013     1     1     554     558     740     728
#>  7      -5  2013     1     1     555     600     913     854
#>  8      -3  2013     1     1     557     600     709     723
#>  9      -3  2013     1     1     557     600     838     846
#> 10      -2  2013     1     1     558     600     753     745
#> # … with 336,766 more rows, 11 more variables:
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​dep_delay, ²​dep_time, ³​sched_dep_time,
#> #   ⁴​arr_time, ⁵​sched_arr_time
```



### mutate() Function {-}

It’s often useful to add new columns that are functions of existing columns. That's what the `mutate()` function does. 

`mutate()` always adds new columns at the end of your data set so we’ll start by creating a narrower data set so we can see the new variables:


```r

flights_2 <- select(flights, month, ends_with("delay"), distance, air_time)
```


Now let's add "gain" and "speed" columns to the data frame:


```r

mutate(flights_2, gain = dep_delay - arr_delay, speed = distance / air_time * 60)
#> # A tibble: 336,776 × 7
#>    month dep_delay arr_delay distance air_time  gain speed
#>    <int>     <int>     <int>    <int>    <int> <int> <dbl>
#>  1     1         2        11     1400      227    -9  370.
#>  2     1         4        20     1416      227   -16  374.
#>  3     1         2        33     1089      160   -31  408.
#>  4     1        -1       -18     1576      183    17  517.
#>  5     1        -6       -25      762      116    19  394.
#>  6     1        -4        12      719      150   -16  288.
#>  7     1        -5        19     1065      158   -24  404.
#>  8     1        -3       -14      229       53    11  259.
#>  9     1        -3        -8      944      140     5  405.
#> 10     1        -2         8      733      138   -10  319.
#> # … with 336,766 more rows
```

Note that you can refer to columns that you've just created:


```r

mutate(flights_2, gain = dep_delay - arr_delay, hours = air_time/60, gain_per_hour = gain/hours)
#> # A tibble: 336,776 × 8
#>    month dep_d…¹ arr_d…² dista…³ air_t…⁴  gain hours gain_…⁵
#>    <int>   <int>   <int>   <int>   <int> <int> <dbl>   <dbl>
#>  1     1       2      11    1400     227    -9 3.78    -2.38
#>  2     1       4      20    1416     227   -16 3.78    -4.23
#>  3     1       2      33    1089     160   -31 2.67   -11.6 
#>  4     1      -1     -18    1576     183    17 3.05     5.57
#>  5     1      -6     -25     762     116    19 1.93     9.83
#>  6     1      -4      12     719     150   -16 2.5     -6.4 
#>  7     1      -5      19    1065     158   -24 2.63    -9.11
#>  8     1      -3     -14     229      53    11 0.883   12.5 
#>  9     1      -3      -8     944     140     5 2.33     2.14
#> 10     1      -2       8     733     138   -10 2.3     -4.35
#> # … with 336,766 more rows, and abbreviated variable names
#> #   ¹​dep_delay, ²​arr_delay, ³​distance, ⁴​air_time,
#> #   ⁵​gain_per_hour
```

If you only want to keep the new variable, use `transmute()` function:


```r

transmute(flights_2, gain = dep_delay - arr_delay, hours = air_time/60, gain_per_hour = gain/hours)
#> # A tibble: 336,776 × 3
#>     gain hours gain_per_hour
#>    <int> <dbl>         <dbl>
#>  1    -9 3.78          -2.38
#>  2   -16 3.78          -4.23
#>  3   -31 2.67         -11.6 
#>  4    17 3.05           5.57
#>  5    19 1.93           9.83
#>  6   -16 2.5           -6.4 
#>  7   -24 2.63          -9.11
#>  8    11 0.883         12.5 
#>  9     5 2.33           2.14
#> 10   -10 2.3           -4.35
#> # … with 336,766 more rows
```


### `%>%` Pipe Operator {-}

The dplyr functions are functional in the sense that function calls don’t have side-effects. You must always save their results. This doesn’t lead to particularly elegant code, especially if you want to do many operations at once. You either have to do it step-by-step or if you don’t want to name the intermediate results, you need to wrap the function calls inside each other, which lead to a messy and complex code:


```r

select(filter(flights, month == 11 | month == 12), starts_with("sched") & ends_with("time"))
#> # A tibble: 55,403 × 2
#>    sched_dep_time sched_arr_time
#>             <int>          <int>
#>  1           2359            345
#>  2           2250           2356
#>  3            500            651
#>  4            545            827
#>  5            545            855
#>  6            600            923
#>  7            600            659
#>  8            600            701
#>  9            600            827
#> 10            600            751
#> # … with 55,393 more rows
```

This is difficult to read because the order of the operations is from inside to out. Thus, the arguments are a long way away from the function. To get around this problem, dplyr provides the `%>%` operator. The pipe operator, `%>%`, comes from the **magrittr** package by Stefan Milton Bache. Packages in the tidyverse load `%>%` for you automatically, so you don't usually load magrittr explicitly. 


`x %>% f(y)` turns into `f(x, y)` so you can use it to rewrite multiple operations that you can read left-to-right, top-to-bottom (reading the pipe operator as “then”):



```r

flights %>% 
  
  filter(month == 11 | month == 12) %>%
  
  select( starts_with("sched") & ends_with("time"))
#> # A tibble: 55,403 × 2
#>    sched_dep_time sched_arr_time
#>             <int>          <int>
#>  1           2359            345
#>  2           2250           2356
#>  3            500            651
#>  4            545            827
#>  5            545            855
#>  6            600            923
#>  7            600            659
#>  8            600            701
#>  9            600            827
#> 10            600            751
#> # … with 55,393 more rows
```


Try to understand what the following code is doing:



```r

flights %>% 
  
  filter(month %in% c(10, 11, 12), arr_delay < 10) %>%
  
  slice(1:30) %>%
  
  arrange(desc(arr_delay)) %>%
  
  select(-c(1,4))
#> # A tibble: 30 × 17
#>    month   day sched_dep_t…¹ dep_d…² arr_t…³ sched…⁴ arr_d…⁵
#>    <int> <int>         <int>   <int>   <int>   <int>   <int>
#>  1    10     1           600      -9     727     730      -3
#>  2    10     1           600      -2     743     751      -8
#>  3    10     1           600     -10     649     659     -10
#>  4    10     1           610      -7     735     745     -10
#>  5    10     1           600      -9     710     721     -11
#>  6    10     1           600      -2     650     701     -11
#>  7    10     1           600      -1     719     730     -11
#>  8    10     1           600       0     706     717     -11
#>  9    10     1           600     -10     648     700     -12
#> 10    10     1           600      -9     655     708     -13
#> # … with 20 more rows, 10 more variables: carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, and abbreviated variable
#> #   names ¹​sched_dep_time, ²​dep_delay, ³​arr_time,
#> #   ⁴​sched_arr_time, ⁵​arr_delay
```


