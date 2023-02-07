# Lecture 07 {-}

&nbsp;

## Importing/Exporting Data into/from R {-}

### Importing Data into R {-}

Working with small examples given in the class is a good way to learn basic functionalities of R, but at some point you want to stop learning and start working with your own data. In this class, we will learn how to import your data (**reading data**) into R and how to save your data (**writing data**) in your computer after you are done with it. 


In R, we can read data from files stored outside the R environment. We can also write data into files which will be stored and accessed by the operating system. R can read and write into various file formats like _csv_, _excel_, _txt_ etc. We will be working mostly with tabular data (data given in rows and columns). There are many function that you can use to import data into R, but we will consider the following two functions: `read.table()` and `read.csv()`.


#### read.table() {-}


One of the most commonly used functions for importing data into R is the `read.table()` function (we discuss some alternatives later in the class). The `read.table()` function is a very flexible function with a shed load of arguments, but it’s quite simple to use. The `read.table()` function has a few important arguments:

* `file` - the name of file, or a connection
* `header` - logical indicating if the file has a header line (it is used to specify column names of a data frame)
* `sep` - a string indicating how the columns are separated (for example, `""`, `","`, `"\t"`, `"/"`)
* `stringAsFactors` - logical indicating whether character variables should be coded as factors.


Now let's import a data set `lung_capacity.txt` available on Courseworks (download it to your computer). In order to import this data into R, we need the following information: a path to this file (its location in your computer), whether it has column names (our data does have column names, so we will pass `TRUE` to the `header` argument, `header = TRUE`), and column separation method (columns of this data were created by separating them with white space, so we will use `sep = ""`):


```r

data1 <- read.table(file = "C:/Users/alexp/OneDrive/Desktop/lung_capacity.txt", header = T, sep = "")
```


Now your data is imported into R and available in the global environment. To view it, use `View()` function:


```r

View(data1)

```

If your data is stored in your working directory, then you can simply pass the name your file without specifying the exact path to it. To see where your working directory is located, use `getwd()` function:


```r

getwd()
#> [1] "C:/Users/alexp/OneDrive/Desktop/2102/2102"
```

If you want to change the location of your working directory, do it by passing a path to this location to the `setwd()` function:



```r

setwd("C:/Users/alexp/OneDrive/Desktop")
```

Now the working directory is my desktop. So any file stored on my desktop can be imported into R by passing just their name to the `read.table()` function:


```r

data1 <- read.table(file = "lung_capacity.txt", header = T, sep = "")

```


#### read.csv() {-}

The file that you are trying to import into R can be comma-delimited (columns are separated with comma). Normally, these files have the `.csv` file extensions. This is a common file extension and statisticians use it frequently, so R has a separate function that allows you to import such files. This function is called `read.csv()`:


```r

data2 <- read.csv(file = "C:/Users/alexp/OneDrive/Desktop/lung_capacity_2.csv", header = T)

```

As you can see, you are no longer required to specify `sep` argument as R already knows that this is comma-delimited file. Thus, there two ways of importing `.csv` files into R: 1) using `read.table()` function with `sep = ","` 2) using `read.csv()` function directly.

It is up to you which method you will choose.

### Exporting Data from R {-}

#### write.table() {-}

After manipulating your data in R (for example, adding or removing columns/row, modifying existing columns, and so on), you might want to save the modified data in your computer. You can do so using `write.table()` function, which has the following arguments:

* `x` - name of an object that you are trying to save
* `file` - name and extension of a file under which your data will be stored in your computer
* `col.names` - logical indicating if the file should contain column names (almost always you would want to assign `TRUE` to this argument)
* `row.names` - logical indicating if the file should contain row index (almost always you would want to assign `FALSE` to this argument)
* `sep` - a string indicating how the columns are separated (for example, `""`, `","`, `"\t"`, `"/"`)

For instance, let's save the `data2` data set in your computer under `new_data` name:


```r

write.table(data2, file = "new_data.csv", col.names = TRUE, row.names = FALSE, sep = ",")

```

#### write.csv() {-}

Like `read.csv()` function, you can use `write.csv()` function to save your data with `csv` extension (comma-delimited file):


```r

write.csv(data2, file = "new_data_2.csv", row.names = FALSE)

```

Now, you don't need to specify `sep` argument as R already knows what type of file it should create.

***

&nbsp;

## Basic Statistics Built-in Functions {-}

R has built-in functions for a large number of `summary statistics`. In this class, we will consider summary statistics for numerical/quantitative variables. Let's use the `Age` variable from the `data1` data set that we've already imported into R.


### Summary {-}

`summary()` function provides basic summary statistics for numerical data:


```r

summary(data1$Age)
#>    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#>    3.00    9.00   13.00   12.33   15.00   19.00
```

### Central Tendency {-}

#### Mean {-}

As the name suggests, `mean()` function computes a mean value of numerical data:


```r

mean(data1$Age)
#> [1] 12.3269
```

Type `?mean()` in the console to see what other arguments `mean()` function has.


#### Median {-}

`median()` function computes a median of a numerical variable:


```r

median(data1$Age)
#> [1] 13
```


### Spread {-}

#### Variance {-}

`var()` function computes a variance of a numerical variable:



```r

var(data1$Age)
#> [1] 16.03802
```


#### Standard Deviation {-}

`sd()` function computes a standard deviation of a numerical variable:


```r

sd(data1$Age)
#> [1] 4.00475
```

Another way of computing a standard deviation is taking a square root of the variance:


```r

sqrt(var(data1$Age))
#> [1] 4.00475
```

#### Quantiles {-}

You can use `quantile()` function to calculate quantiles (a.k.a quartiles, a.k.a percentiles) of a numerical variable. By default, it gives you basic quantiles:


```r

quantile(data1$Age)
#>   0%  25%  50%  75% 100% 
#>    3    9   13   15   19
```

But you can make `quantile()` function compute specific quantiles. For examples, let's compute 0.30 and 0.80 quantiles (that is, 30-th and 80-th percentiles):


```r

quantile(data1$Age, c(0.3, 0.8))
#> 30% 80% 
#>  10  16
```


##### Inverse Quantiles {-}

Suppose given an observation `x` from your data, you want to know its corresponding quantile. That is, you want to know what fraction of the data is less than `x`. For example, let's find what is the corresponding quantile for 11 in the `Age` variable:


```r

mean(data1$Age < 11)
#> [1] 0.3213793
```


The expression `data1$Age < 11` compares every element of the `Age` variable against `11` and returns a vector of logical values, where the _n_-th logical value is `TRUE` if `data1$Age[n] < 11`. The mean function converts those logical values to `0` and `1`: `0` for `FALSE` and `1` for `TRUE`. The average of all those `1`s and `0`s is the fraction of the `Age` variable that is less than `11`, or the inverse quantile of `11`.


#### IQR (Inter-Quartile Range) {-}

IQR is the difference between Q3 (third quartile, 75-th percentile) and Q1 (first quartile, 25-th percentile):


```r

IQR(data1$Age)
#> [1] 6
```

Another way of computing IQR is using `quantile()` functions:



```r

quantile(data1$Age, 0.75) - quantile(data1$Age, 0.25)
#> 75% 
#>   6
```


#### Minimum {-}

`min()` function calculates a minimum value of a numerical variable:



```r

min(data1$Age)
#> [1] 3
```

##### which.min() {-}

You can use `which.min()` function to find the index position of the minimum value in the variable:


```r

which.min(data1$Age)
#> [1] 114
```

#### Maximum {-}

`max()` function calculates a maximum value of a numerical variable:



```r

max(data1$Age)
#> [1] 19
```

##### which.max() {-}

You can use `which.max()` function to find the index position of the maximum value in the variable:


```r

which.max(data1$Age)
#> [1] 11
```

#### Range {-}

Another way of calculating a maximum and a minimum of a numerical variable is using `range()` function.


```r

range(data1$Age)
#> [1]  3 19
```


By definition, _range_ is the difference between the maximum and minimum values of a numerical variable. To compute this quantity, we can utilize `max()` and `min()` functions:


```r

max(data1$Age) - min(data1$Age)
#> [1] 16
```


#### Summation {-}

`sum()` function adds up the elements of a numerical variable:


```r

sum(data1$Age)
#> [1] 8937
```

Let's use `sum()` and `length()` functions to compute the mean value:


```r

sum(data1$Age)/length(data1$Age)
#> [1] 12.3269
```

##### Cumulative Summation {-}

`cumsum()` function produces a vector containing the cumulative sum of the input vector (that is, it adds the values up sequentially displaying the results after each addition). For simplicity, let's use the following vector:


```r

x <- c(2, 4, 3, 5, 10)

print(x)
#> [1]  2  4  3  5 10
```


So the cumulative summation of the vector `x` would be:


```r

cumsum(x)
#> [1]  2  6  9 14 24
```


***

&nbsp;

## Functional Programming: apply() Family of Functions {-}

Writing `for` and `while` loops is useful when programming but not particularly easy when working interactively on the command line. Multi-line expressions with curly braces are just not that easy to sort through when working on the command line.

Moreover, `for` loops have a bad reputation in R because many people believe they are slow, but the real downside of `for` loops is that they’re not very flexible: a loop conveys that you’re iterating, but not what should be done with the results.

Fortunately, R has built-in functions that overcome aforementioned challenges. They  implement looping in a compact form to make your life easier. In this class, we will consider several such functions that can be combined into a family of function called `apply()` family of function.

`apply()` functions use functional programming, which implies using functions as arguments for other functions (yeah, too many functions). Let's start with basic ones:

### apply() Function{-}

The `apply()` function enables applying a function to the rows or columns of a matrix or data frame. Depending on what function you specify when using the apply function, you will get back either a vector or a matrix. The general structure of the `apply()` function is as follows:


```r

apply(X, MARGIN, FUN, ...)

```


* `X` - is the object you pass to this function (either a matrix or data frame)
* `MARGIN` - this argument uses values `1` or `2`, where `1` is for rows and `2` is for columns.
* `FUN` - is a function or command that you want to apply to either rows or columns
* `...` - you can add additional instructions if they are appropriate to the command/function you are applying


For example, let's create a matrix with `5x4` dimensions:


```r

matrix1 <- matrix(1:20, nrow = 5, byrow = T)

print(matrix1)
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3    4
#> [2,]    5    6    7    8
#> [3,]    9   10   11   12
#> [4,]   13   14   15   16
#> [5,]   17   18   19   20
```

Now, suppose you want to calculate the mean value of each column in this matrix. You can do so using the `apply()` function:


```r

apply(matrix1, 2, mean)
#> [1]  9 10 11 12
```


We can do the same thing row-wise:


```r

apply(matrix1, 1, mean)
#> [1]  2.5  6.5 10.5 14.5 18.5
```


We can even pass a user-defined function. For example, first let's create a function that calculates a range:



```r

function_1 <- function(x){
  
  range <- max(x) - min(x)
  
  return(range)
}
```

Now, let's apply this function to columns of the `matrix1`:



```r

apply(matrix1, 2, function_1)
#> [1] 16 16 16 16
```


### lapply() Function {-}

If you use `apply()` function with lists, it will throw an error. Suppose we have a list:



```r

list1 <- list(c(1, 4, 10), 24, 1:5)

print(list1)
#> [[1]]
#> [1]  1  4 10
#> 
#> [[2]]
#> [1] 24
#> 
#> [[3]]
#> [1] 1 2 3 4 5
```

And now we want to calculate the length of each element in `list1`. If you use `apply()` function, it will cause an error:


```r

apply(list1, 2, length)
#> Error in apply(list1, 2, length): dim(X) must have a positive length
```


Instead use `lapply()` function:


```r

lapply(list1, length)
#> [[1]]
#> [1] 3
#> 
#> [[2]]
#> [1] 1
#> 
#> [[3]]
#> [1] 5
```


### sapply() Function {-}

As you noticed, the previous command returned a list. If you want your output to be a vector, use `sapply()` function:


```r

sapply(list1, length)
#> [1] 3 1 5
```


### mapply() Function {-}

The `mapply()` function is a multivariate `apply() function`  which applies a function in parallel over a set of arguments. It is used to iterate over multiple R objects in parallel. The `mapply()` function has a different argument order from `lapply()` because the function to apply comes first rather than the object to iterate over. For example, you want to create the following list:


```r

list(rep(1, 4), rep(2, 3), rep(3, 2), rep(4, 1))
#> [[1]]
#> [1] 1 1 1 1
#> 
#> [[2]]
#> [1] 2 2 2
#> 
#> [[3]]
#> [1] 3 3
#> 
#> [[4]]
#> [1] 4
```

Instead, you can use `mapply()` function as follows:


```r

mapply(rep, 1:4, 4:1)
#> [[1]]
#> [1] 1 1 1 1
#> 
#> [[2]]
#> [1] 2 2 2
#> 
#> [[3]]
#> [1] 3 3
#> 
#> [[4]]
#> [1] 4
```
