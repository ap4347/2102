
# (PART) TIDYVERSE {-}

# Lecture 10 {-}

&nbsp;


## Tidyverse Family of Packages {-}

**_Data frame_** is a key data structure in statistics and in R. The basic structure of a data frame is that there is one observation per row and each column represents a variable, a measure, feature, or characteristic of that observation. Before you can conduct any analyses or draw any conclusions, you often need to reorganize your data. The **Tidyverse** is a collection of R packages (developed by RStudio’s chief scientist Hadley Wickham) that provides an efficient, fast, and well-documented workflow for general data modeling, wrangling, and visualization tasks.


The Tidyverse introduces a set of useful data analysis packages to help streamline your work in R. In particular, the Tidyverse was designed to address the top three common issues that arise when dealing with data analysis in  base R: (1) Results obtained from a base R function often depend on the type of data being used; (2) When R expressions are used in a non-standard way, they can confuse beginners; (3) Hidden arguments often have various default operations that beginners are unaware of.


The core Tidyverse includes the packages that you’re likely to use in everyday data analyses:

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


``` r

install.packages("tidyverse")

```


Now the Tidyverse is available in R, but it is not activated yet. Whenever you start a new R session and plan to use the Tidyverse packages, you will need to activate the package by calling the `library(tidyverse)` function in the console:


``` r

library(tidyverse)
#> ── Attaching core tidyverse packages ──── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.4     ✔ readr     2.1.5
#> ✔ forcats   1.0.0     ✔ stringr   1.5.1
#> ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
#> ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
#> ✔ purrr     1.0.4     
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```


Today we will start learning the Tidyverse family of packages by introducing the `dplyr` package.


We will be working with `nyc_flights` data set that provides information about flights departed New York City in 2013 (the data set is available on Courseworks). It contains 336 776 observations (rows) and 19 variables (columns). Let's import this data set into R: 



``` r

flights <- read.csv(file = "C:/Users/alexp/OneDrive/Documents/nyc_flights.csv", header = T)

```

Let's convert our data frame into a `tibble` data frame (we will discuss this format in details later in the semester):


``` r

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



``` r

filter(flights, month == 1, day == 1)
#> # A tibble: 842 × 19
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
#> # ℹ 832 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

When you run that line of code, dplyr executes the filtering operation and returns a new data frame. dplyr functions never modify their inputs, so if you want to save the result, you’ll need to use the assignment operator, `<-` :


``` r

jan1 <- filter(flights, month == 1, day == 1)

```


Let's find all flights that departed in November or December:


``` r

filter(flights, month == 11 | month == 12)
#> # A tibble: 55,403 × 19
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013    11     1        5           2359         6
#>  2  2013    11     1       35           2250       105
#>  3  2013    11     1      455            500        -5
#>  4  2013    11     1      539            545        -6
#>  5  2013    11     1      542            545        -3
#>  6  2013    11     1      549            600       -11
#>  7  2013    11     1      550            600       -10
#>  8  2013    11     1      554            600        -6
#>  9  2013    11     1      554            600        -6
#> 10  2013    11     1      554            600        -6
#> # ℹ 55,393 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

We could do the same operation using the `%in%` operator:


``` r

filter(flights, month %in% c(11, 12))
#> # A tibble: 55,403 × 19
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013    11     1        5           2359         6
#>  2  2013    11     1       35           2250       105
#>  3  2013    11     1      455            500        -5
#>  4  2013    11     1      539            545        -6
#>  5  2013    11     1      542            545        -3
#>  6  2013    11     1      549            600       -11
#>  7  2013    11     1      550            600       -10
#>  8  2013    11     1      554            600        -6
#>  9  2013    11     1      554            600        -6
#> 10  2013    11     1      554            600        -6
#> # ℹ 55,393 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


### slice() Function {-}

`slice()` function allows you to index rows by their (integer) locations. It can select, remove, and duplicate rows.

For instance, let's get observations from rows 5 through 10:


``` r

slice(flights, 5:10)
#> # A tibble: 6 × 19
#>    year month   day dep_time sched_dep_time dep_delay
#>   <int> <int> <int>    <int>          <int>     <int>
#> 1  2013     1     1      554            600        -6
#> 2  2013     1     1      554            558        -4
#> 3  2013     1     1      555            600        -5
#> 4  2013     1     1      557            600        -3
#> 5  2013     1     1      557            600        -3
#> 6  2013     1     1      558            600        -2
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


Let's select all rows except the first four (this option can be used to drop some observations from a data set):


``` r

slice(flights, -(1:4))
#> # A tibble: 336,772 × 19
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013     1     1      554            600        -6
#>  2  2013     1     1      554            558        -4
#>  3  2013     1     1      555            600        -5
#>  4  2013     1     1      557            600        -3
#>  5  2013     1     1      557            600        -3
#>  6  2013     1     1      558            600        -2
#>  7  2013     1     1      558            600        -2
#>  8  2013     1     1      558            600        -2
#>  9  2013     1     1      558            600        -2
#> 10  2013     1     1      558            600        -2
#> # ℹ 336,762 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


Similar to `head()` and `tail()` functions, `slice_head()` and `slice_tail()` can be used to display top and bottom rows in the data set, respectively. Let's print first and last 3 rows in the flights data set:


``` r

slice_head(flights, n = 3)
#> # A tibble: 3 × 19
#>    year month   day dep_time sched_dep_time dep_delay
#>   <int> <int> <int>    <int>          <int>     <int>
#> 1  2013     1     1      517            515         2
#> 2  2013     1     1      533            529         4
#> 3  2013     1     1      542            540         2
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


``` r

slice_tail(flights, n = 3)
#> # A tibble: 3 × 19
#>    year month   day dep_time sched_dep_time dep_delay
#>   <int> <int> <int>    <int>          <int>     <int>
#> 1  2013     9    30       NA           1210        NA
#> 2  2013     9    30       NA           1159        NA
#> 3  2013     9    30       NA            840        NA
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

Use the `slice_sample()` function to randomly select rows. Use the option `prop` to choose a certain proportion of the cases:


``` r

slice_sample(flights, n = 10)
#> # A tibble: 10 × 19
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013     2    24      808            800         8
#>  2  2013    11    12     1853           1900        -7
#>  3  2013    11    11     1722           1428       174
#>  4  2013     5    25      811            815        -4
#>  5  2013    11    23      602            611        -9
#>  6  2013    10    30     1819           1820        -1
#>  7  2013     6    27     1354           1400        -6
#>  8  2013     6    25     1627           1630        -3
#>  9  2013     8     6     1614           1620        -6
#> 10  2013     1    15     1126           1129        -3
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


``` r

slice_sample(flights, prop = 0.001)
#> # A tibble: 336 × 19
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013    10    31     1029           1030        -1
#>  2  2013     4     9      604            608        -4
#>  3  2013    10    23     1558           1600        -2
#>  4  2013    11    15     1621           1630        -9
#>  5  2013     3    26     2221           2225        -4
#>  6  2013    10     6      907            910        -3
#>  7  2013     7     8     1326           1330        -4
#>  8  2013     6    13     1714           1629        45
#>  9  2013    12     3      756            800        -4
#> 10  2013     9     4     1432           1438        -6
#> # ℹ 326 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

Use `replace = TRUE` to take a sample with replacement. 


### arrange() Function {-}

The `arrange()` function is used to change the order of rows in a data set. It takes a data frame and a set of column names (or more complicated expressions) to order by. If you provide more than one column name, each additional column will be used to break ties in the values of preceding columns:


``` r

arrange(flights, year, month, day)
#> # A tibble: 336,776 × 19
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
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

Use `desc()` to re-order by a column in descending order:


``` r

arrange(flights, desc(dep_delay))
#> # A tibble: 336,776 × 19
#>     year month   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1  2013     1     9      641            900      1301
#>  2  2013     6    15     1432           1935      1137
#>  3  2013     1    10     1121           1635      1126
#>  4  2013     9    20     1139           1845      1014
#>  5  2013     7    22      845           1600      1005
#>  6  2013     4    10     1100           1900       960
#>  7  2013     3    17     2321            810       911
#>  8  2013     6    27      959           1900       899
#>  9  2013     7    22     2257            759       898
#> 10  2013    12     5      756           1700       896
#> # ℹ 336,766 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```



### select() Function {-}

Often you work with large data sets with many columns but only a few are actually of interest to you. `select()` function allows you to rapidly zoom in on a useful subset. You can select columns by name:


``` r

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
#> # ℹ 336,766 more rows
```

You can select all columns between two variables (inclusive):


``` r

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
#> # ℹ 336,766 more rows
```

You can select all columns except some:


``` r

select(flights, -(year:day))
#> # A tibble: 336,776 × 16
#>    dep_time sched_dep_time dep_delay arr_time sched_arr_time
#>       <int>          <int>     <int>    <int>          <int>
#>  1      517            515         2      830            819
#>  2      533            529         4      850            830
#>  3      542            540         2      923            850
#>  4      544            545        -1     1004           1022
#>  5      554            600        -6      812            837
#>  6      554            558        -4      740            728
#>  7      555            600        -5      913            854
#>  8      557            600        -3      709            723
#>  9      557            600        -3      838            846
#> 10      558            600        -2      753            745
#> # ℹ 336,766 more rows
#> # ℹ 11 more variables: arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

You can do the same operation with `!` operator:


``` r

select(flights, !(year:day))
#> # A tibble: 336,776 × 16
#>    dep_time sched_dep_time dep_delay arr_time sched_arr_time
#>       <int>          <int>     <int>    <int>          <int>
#>  1      517            515         2      830            819
#>  2      533            529         4      850            830
#>  3      542            540         2      923            850
#>  4      544            545        -1     1004           1022
#>  5      554            600        -6      812            837
#>  6      554            558        -4      740            728
#>  7      555            600        -5      913            854
#>  8      557            600        -3      709            723
#>  9      557            600        -3      838            846
#> 10      558            600        -2      753            745
#> # ℹ 336,766 more rows
#> # ℹ 11 more variables: arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```

You can use column indexes for column selection:


``` r

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
#> # ℹ 336,766 more rows
```

There are a number of helper functions you can use within `select()`. For example, `starts_with()`, `ends_with()`, `matches()` and `contains()`. These let you quickly match larger blocks of variables that meet some criterion.

Let's select all columns that start with "sched":


``` r

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
#> # ℹ 336,766 more rows
```

You can select all columns in the data set that end with "time":


``` r

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
#> # ℹ 336,766 more rows
```

Or suppose you want to select all columns in the data set that contain "ar":


``` r

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
#> # ℹ 336,766 more rows
```

You can even combine these arguments:


``` r

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
#> # ℹ 336,766 more rows
```


### rename() Function {-}

Use `rename()` function to rename columns in a data frame. Suppose we want to rename the "year" and "month" variables and make them uppercase:


``` r

rename(flights, YEAR = year, MONTH = month)
#> # A tibble: 336,776 × 19
#>     YEAR MONTH   day dep_time sched_dep_time dep_delay
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
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


### relocate() Function {-}

`relocate()` function allows to change the positions of columns in a data frame. It has two useful arguments `.before` and `.after` that helps precisely select a location for a variable:


``` r

relocate(flights, year, .after = month)
#> # A tibble: 336,776 × 19
#>    month  year   day dep_time sched_dep_time dep_delay
#>    <int> <int> <int>    <int>          <int>     <int>
#>  1     1  2013     1      517            515         2
#>  2     1  2013     1      533            529         4
#>  3     1  2013     1      542            540         2
#>  4     1  2013     1      544            545        -1
#>  5     1  2013     1      554            600        -6
#>  6     1  2013     1      554            558        -4
#>  7     1  2013     1      555            600        -5
#>  8     1  2013     1      557            600        -3
#>  9     1  2013     1      557            600        -3
#> 10     1  2013     1      558            600        -2
#> # ℹ 336,766 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


``` r

relocate(flights, c(year, month), .before = dep_delay)
#> # A tibble: 336,776 × 19
#>      day dep_time sched_dep_time  year month dep_delay
#>    <int>    <int>          <int> <int> <int>     <int>
#>  1     1      517            515  2013     1         2
#>  2     1      533            529  2013     1         4
#>  3     1      542            540  2013     1         2
#>  4     1      544            545  2013     1        -1
#>  5     1      554            600  2013     1        -6
#>  6     1      554            558  2013     1        -4
#>  7     1      555            600  2013     1        -5
#>  8     1      557            600  2013     1        -3
#>  9     1      557            600  2013     1        -3
#> 10     1      558            600  2013     1        -2
#> # ℹ 336,766 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```



``` r

relocate(flights, c(year, month), .after = last_col())
#> # A tibble: 336,776 × 19
#>      day dep_time sched_dep_time dep_delay arr_time
#>    <int>    <int>          <int>     <int>    <int>
#>  1     1      517            515         2      830
#>  2     1      533            529         4      850
#>  3     1      542            540         2      923
#>  4     1      544            545        -1     1004
#>  5     1      554            600        -6      812
#>  6     1      554            558        -4      740
#>  7     1      555            600        -5      913
#>  8     1      557            600        -3      709
#>  9     1      557            600        -3      838
#> 10     1      558            600        -2      753
#> # ℹ 336,766 more rows
#> # ℹ 14 more variables: sched_arr_time <int>,
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>, year <int>, month <int>
```


``` r

relocate(flights, dep_delay, .before = everything())
#> # A tibble: 336,776 × 19
#>    dep_delay  year month   day dep_time sched_dep_time
#>        <int> <int> <int> <int>    <int>          <int>
#>  1         2  2013     1     1      517            515
#>  2         4  2013     1     1      533            529
#>  3         2  2013     1     1      542            540
#>  4        -1  2013     1     1      544            545
#>  5        -6  2013     1     1      554            600
#>  6        -4  2013     1     1      554            558
#>  7        -5  2013     1     1      555            600
#>  8        -3  2013     1     1      557            600
#>  9        -3  2013     1     1      557            600
#> 10        -2  2013     1     1      558            600
#> # ℹ 336,766 more rows
#> # ℹ 13 more variables: arr_time <int>,
#> #   sched_arr_time <int>, arr_delay <int>, carrier <chr>,
#> #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```



### mutate() Function {-}

It’s often useful to add new columns that are functions of existing columns. That's what the `mutate()` function does. 

`mutate()` always adds new columns at the end of your data set so we’ll start by creating a narrower data set so we can see the new variables:


``` r

flights_2 <- select(flights, month, ends_with("delay"), distance, air_time)

```


Now let's add "gain" and "speed" columns to the data frame:


``` r

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
#> # ℹ 336,766 more rows
```

Note that you can refer to columns that you've just created:


``` r

mutate(flights_2, gain = dep_delay - arr_delay, hours = air_time/60, gain_per_hour = gain/hours)
#> # A tibble: 336,776 × 8
#>    month dep_delay arr_delay distance air_time  gain hours
#>    <int>     <int>     <int>    <int>    <int> <int> <dbl>
#>  1     1         2        11     1400      227    -9 3.78 
#>  2     1         4        20     1416      227   -16 3.78 
#>  3     1         2        33     1089      160   -31 2.67 
#>  4     1        -1       -18     1576      183    17 3.05 
#>  5     1        -6       -25      762      116    19 1.93 
#>  6     1        -4        12      719      150   -16 2.5  
#>  7     1        -5        19     1065      158   -24 2.63 
#>  8     1        -3       -14      229       53    11 0.883
#>  9     1        -3        -8      944      140     5 2.33 
#> 10     1        -2         8      733      138   -10 2.3  
#> # ℹ 336,766 more rows
#> # ℹ 1 more variable: gain_per_hour <dbl>
```

If you only want to keep the new variable, use `transmute()` function:


``` r

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
#> # ℹ 336,766 more rows
```


### `%>%` Pipe Operator {-}

The dplyr functions are functional in the sense that function calls don’t have side-effects. You must always save their results. This doesn’t lead to particularly elegant code, especially if you want to do many operations at once. You either have to do it step-by-step or if you don’t want to name the intermediate results, you need to wrap the function calls inside each other, which lead to a messy and complex code:


``` r

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
#> # ℹ 55,393 more rows
```

This is difficult to read because the order of the operations is from inside to out. Thus, the arguments are a long way away from the function. To get around this problem, dplyr provides the `%>%` operator. The pipe operator, `%>%`, comes from the **magrittr** package by Stefan Milton Bache. Packages in the tidyverse load `%>%` for you automatically, so you don't usually load magrittr explicitly. 


`x %>% f(y)` turns into `f(x, y)` so you can use it to rewrite multiple operations that you can read left-to-right, top-to-bottom (reading the pipe operator as “then”):



``` r

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
#> # ℹ 55,393 more rows
```


Try to understand what the following code is doing:



``` r

flights %>% 
  
  filter(month %in% c(10, 11, 12), arr_delay < 10) %>%
  
  slice(1:30) %>%
  
  arrange(desc(arr_delay)) %>%
  
  select(-c(1,4))
#> # A tibble: 30 × 17
#>    month   day sched_dep_time dep_delay arr_time
#>    <int> <int>          <int>     <int>    <int>
#>  1    10     1            600        -9      727
#>  2    10     1            600        -2      743
#>  3    10     1            600       -10      649
#>  4    10     1            610        -7      735
#>  5    10     1            600        -9      710
#>  6    10     1            600        -2      650
#>  7    10     1            600        -1      719
#>  8    10     1            600         0      706
#>  9    10     1            600       -10      648
#> 10    10     1            600        -9      655
#> # ℹ 20 more rows
#> # ℹ 12 more variables: sched_arr_time <int>,
#> #   arr_delay <int>, carrier <chr>, flight <int>,
#> #   tailnum <chr>, origin <chr>, dest <chr>,
#> #   air_time <int>, distance <int>, hour <int>,
#> #   minute <int>, time_hour <chr>
```


