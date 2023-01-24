# Lecture 02 {-}



## Data Types {-}

In programming, data types is an important concept. Variables can store data of different types, and different types can do different things. For correct processing, a programming language must know what can and cannot be done to a particular value. For example, addition cannot be preformed on the words `Hello` and `world`. Similarly, you cannot change the numbers `5` or `-22` from lower to uppercase. 


Due to this, R has a feature called the `data types`. Different kinds of values are assigned different data types that help differentiate them. These types have certain characteristics and rules associated with them that define their properties.


In this course we will consider the following data types:


* Numeric
* Integers
* Complex
* Logical
* Characters

There are more data types available in R, but it is beyond the scope of this class. Let's got through these data types one-by-one.


### Numeric Data Type {-}


As you may expect, `numeric data type` is for numerical values. To create a variable of a numeric data types, simply assign any numeric value to the variable.


```r

x_num <- 1

x_num
#> [1] 1
```



```r

y_num <- -2.35

y_num
#> [1] -2.35
```


Use `class()` function to find out what is the type of any variable.



```r

class(x_num)
#> [1] "numeric"

class(y_num)
#> [1] "numeric"
```



### Integers Data Type {-}

An `integers data type` is a special case of the `numeric data type` and is used for integer values. To store a value as an integer, we need to specify it as such using `as.integer()` function:



```r

x_int <- as.integer(2)

x_int
#> [1] 2

class(x_int)
#> [1] "integer"
```

If an input value is not an integer itself (for example, 2.85), `as.integer()` function will remove the decimal points and will keep integers only.



```r

y_int <- as.integer(2.85)

y_int
#> [1] 2

class(x_int)
#> [1] "integer"
```

Another way of creating a variable of the `integer data type` is to use an integer followed by `L` letter:



```r

z_int <- 4L

z_int
#> [1] 4

class(z_int)
#> [1] "integer"
```


### Complex Data Type {-}


`Complex data types` are used to store numbers with an imaginary component. For instance, `1 + 3i`, `2 - 5i`, and `3 - 4i`. In this class we are not going to use this data type, but it is good to know about them.



```r

x_comp <- 20 + 6i

x_comp
#> [1] 20+6i

class(x_comp)
#> [1] "complex"
```



### Logical Data Type {-}

The `logical data type` stores _logical_ (also known as _boolean_) values of `TRUE` and `FALSE`:



```r

x_logical <- TRUE

x_logical
#> [1] TRUE

class(x_logical)
#> [1] "logical"

y_logical <- FALSE

class(y_logical)
#> [1] "logical"

z_logical <- T

class(z_logical)
#> [1] "logical"
```



### Character Data Type {-}

The `character data type` stores character values or strings. Strings in R can contain the alphabet, numbers, and symbols. The easiest way to denote that a value is of `character type` in R is to wrap the value inside single or double quotes:



```r

x_char <- "2102"

x_char
#> [1] "2102"

class(x_char)
#> [1] "character"

y_char <- "Welcome to STAT 2102!"

y_char
#> [1] "Welcome to STAT 2102!"

class(y_char)
#> [1] "character"
```


### Converting Data Types {-}

In R we can convert values from one data type to another. R has certain rules that govern these conversions.


#### Converting into Numeric Data Type {-}

Before we discuss how to convert any other data type into numeric, let's first introduce `is.numeric()` function that checks whether a variable is of numeric data type:


```r

is.numeric(x_num)
#> [1] TRUE

is.numeric(x_char)
#> [1] FALSE
```


To convert any other data type into numeric, we can use `as.numeric()` function. When converting integer type data into numeric, `as.numeric()` changes its type and keeps the value as it is; when converting a complex data type, it removes the imaginary part of the number; when converting logical data type, the `TRUE` value is converted to `1`, and `FALSE` is converted to `0`; finally, character values can similarly be converted into numerical values but if the string contains letters or other symbols then the numeric value becomes `NA`:


```r

######################################
x_comp
#> [1] 20+6i

is.numeric(x_comp)
#> [1] FALSE

num1 <- as.numeric(x_comp)
#> Warning: imaginary parts discarded in coercion

class(num1)
#> [1] "numeric"

num1
#> [1] 20

######################################

x_logical
#> [1] TRUE

logical1 <- as.numeric(x_logical)

class(logical1)
#> [1] "numeric"

logical1
#> [1] 1

######################################

y_logical
#> [1] FALSE

logical2 <- as.numeric(y_logical)

class(logical1)
#> [1] "numeric"

logical2
#> [1] 0

######################################

y_char
#> [1] "Welcome to STAT 2102!"

char1 <- as.numeric(y_char)
#> Warning: NAs introduced by coercion

class(char1)
#> [1] "numeric"

char1
#> [1] NA

######################################

x_char
#> [1] "2102"

char2 <- as.numeric(x_char)

class(char2)
#> [1] "numeric"

char2
#> [1] 2102
```


#### Converting into Integer Data Type {-}

To convert any other data type into integer, we can use `as.integer()` function. The properties of this function are similar to those stated above, so we will skip them here. (Try it yourself!)


#### Converting into Logical Data Type {-}

To convert any other data type into logical, we can utilize `as.logical()` function. It return `FALSE` if the value is zero and `TRUE` if it anything else. Character values when converted by the `as.logical()` function, always return `NA`:


```r

######################################
y_num
#> [1] -2.35

is.logical(y_num)
#> [1] FALSE

logi1 <- as.logical(y_num)

class(logi1)
#> [1] "logical"

logi1
#> [1] TRUE



######################################

y_char
#> [1] "Welcome to STAT 2102!"

logi2 <- as.logical(y_char)

class(logi2)
#> [1] "logical"

logi2
#> [1] NA

######################################

x_char
#> [1] "2102"

logi3 <- as.logical(x_char)

class(logi3)
#> [1] "logical"

logi3
#> [1] NA
```


#### Converting into Character Data Type {-}

We can convert any data type into character data type using the `as.character()` function. It converts the original value into a character string.



```r

######################################
y_num
#> [1] -2.35

is.character(y_num)
#> [1] FALSE

char1 <- as.character(y_num)

class(char1)
#> [1] "character"

char1
#> [1] "-2.35"

######################################

x_comp
#> [1] 20+6i

char2 <- as.character(x_comp)

class(char2)
#> [1] "character"

char2
#> [1] "20+6i"
```



## Data Structures {-}

In any programming language, you need to use different variables to store different data. Unlike other programming languages like `C` and `Java`, R doesn't have variables declared as some data type. Further, the variables are appointed with R-objects and the knowledge form of the R-object becomes the datatype of the variable. There are many types of R-objects (data structures). The commonly used ones are:

* Vectors
* Lists
* Matrices
* Data Frames
* Factors

In this lecture, we will discuss `vectors` and `lists`. Later, we will go over other data structures as well.


### Vectors {-}

#### Creating Vectors {-}

Vector is the most basic data structure in R programming language. There are various ways of **creating a vector**. The most common way is using `c()` function:


```r

vec1 <- c(1, 2, 3, 4, 5)

vec1
#> [1] 1 2 3 4 5

vec2 <- c("fall", "winter", "spring", "summer")

vec2
#> [1] "fall"   "winter" "spring" "summer"
```

You can also use `:` operator to create a vector:


```r

vec3 <- 3:11

vec3
#> [1]  3  4  5  6  7  8  9 10 11
```

Another way is to use `seq()` function:


```r

vec4 <- seq(from = 1, to = 5, by = 0.7)

vec4
#> [1] 1.0 1.7 2.4 3.1 3.8 4.5


vec5 <- seq(from = 1, to = 5, length.out = 8)

vec5
#> [1] 1.000000 1.571429 2.142857 2.714286 3.285714 3.857143
#> [7] 4.428571 5.000000
```


We can consider one more function, `rep()`, to create a vector:


```r

vec6 <- rep(5, times = 3)

vec6
#> [1] 5 5 5

vec7 <- rep(c(1,3,4), times = 2)

vec7
#> [1] 1 3 4 1 3 4

vec8 <- rep(c("apple", "orange", "mango"), times = 2, each = 3)

vec8
#>  [1] "apple"  "apple"  "apple"  "orange" "orange" "orange"
#>  [7] "mango"  "mango"  "mango"  "apple"  "apple"  "apple" 
#> [13] "orange" "orange" "orange" "mango"  "mango"  "mango"
```


#### How many elements does your vector contain? {-}

We can use the `length()` function to check how many elements are stored in vectors:


```r

vec7
#> [1] 1 3 4 1 3 4

length(vec7)
#> [1] 6
```


#### Adding Elements to Vectors {-}

In order to add new elements to an existing vector, we can utilize `c()` function once again:


```r

# Adding three elements, c(15, 3, 4), to vec1

vec9 <- c(vec1, c(15, 3, 4))

vec9
#> [1]  1  2  3  4  5 15  3  4

# Merging vec1 and vec3

vec10 <- c(vec1, vec3)

vec10
#>  [1]  1  2  3  4  5  3  4  5  6  7  8  9 10 11
```

If you would like to insert an element(s) at the specific position(s) in the vector, use `append()` function:


```r

# Insert 55 to vec1 at the 2nd position

vec11 <- append(vec1, 55, after = 1) 

vec11
#> [1]  1 55  2  3  4  5
```


#### Subsetting/Indexing Vectors {-}

We use square brackets, `[]`, to extract specific elements from vectors:


```r

# selects the first element of the vec1

vec1[1]  
#> [1] 1

# selects the 1st, 5th, and 8th elements of the vec9

vec9[c(1,5,8)]  
#> [1] 1 5 4

# selects the 6th, 7th, and 8th elements of the vec9

vec9[4:7] 
#> [1]  4  5 15  3

# selects the first and second elements of vec1

vec1[c(T, T, F, F, F,F)]  
#> [1] 1 2

# select all elements of vec1 that are greater than 2.5

vec1[vec1 > 2.5] 
#> [1] 3 4 5

# select all elements of vec1 that are not equal to 3

vec1[vec1 != 3] 
#> [1] 1 2 4 5

# selects all elements of vec1 except 4th one

vec2[-4]  
#> [1] "fall"   "winter" "spring"

# selects all elements of vec1 except the 1st and 2nd ones

vec2[c(-1, -2)]                
#> [1] "spring" "summer"
```


#### Assigning new values to the elements of the existing vector {-}

Use the assignment sign, `<-`, to assign new values to the elements of the existing vector:



```r

# Assigning a new value to the first element of vec1

vec1
#> [1] 1 2 3 4 5

vec1[1] <- 100

vec1
#> [1] 100   2   3   4   5
```

## Vectorization {-}

The main advantage of vectors in R is that you can perform vectorized operations on them:



```r

# Adding 1 to each element of vec1

vec1 + 1 
#> [1] 101   3   4   5   6

# For each element of the vector (1:3), raising 2 to the power of its elements

2^(1:3) 
#> [1] 2 4 8

# Doing elementwise addition (you can do it with all arithmetic operations)

c(1, 2, 3) + c(4, 5, 6)  
#> [1] 5 7 9

# Be careful! vectors should have the same length

c(1, 2, 3) + c(4, 5, 6, 7)  
#> Warning in c(1, 2, 3) + c(4, 5, 6, 7): longer object length
#> is not a multiple of shorter object length
#> [1] 5 7 9 8

# Checking whether 2 is in vec1 using %in% function

2 %in% vec1
#> [1] TRUE
```


## Vectors are Homogeneous! {-}

The main disadvantage of vectors in R is that they can store homogeneous data only (data of the same type). If the elements of a vector are of different data types, then the vector will convert their types so that all elements are of the same type:




```r

# R will convert all elements of vec12 into characters, because vectors can only 
#contain homogeneous data

vec12 <- c(2, 3.5, "fall", 2.7)   

vec12
#> [1] "2"    "3.5"  "fall" "2.7"

class(vec12)
#> [1] "character"
```

&nbsp;

_Question_: What if I want to store heterogeneous data (data of different types)?


_Solution_: Use `Lists`.



### Lists {-}

#### Creating Lists {-}

You can create a list using `list()` function:



```r

list1 <- list(2, 3.5, "fall", 2.7)

list1
#> [[1]]
#> [1] 2
#> 
#> [[2]]
#> [1] 3.5
#> 
#> [[3]]
#> [1] "fall"
#> 
#> [[4]]
#> [1] 2.7


list2 <- list(c(2,4,10), c("one", "two", "three"), 45)

list2
#> [[1]]
#> [1]  2  4 10
#> 
#> [[2]]
#> [1] "one"   "two"   "three"
#> 
#> [[3]]
#> [1] 45
```



#### Subsetting/Indexing lists using square brackets (single and double), [] and [[]] {-}



```r

# Selecting the first element of the list2 as a list

list2[1] 
#> [[1]]
#> [1]  2  4 10

# Selecting the first element of the list2 as it is

list2[[1]]                                    
#> [1]  2  4 10

# Selecting the second element of the first element of the list2

list2[[1]][2]                                 
#> [1] 4
```


#### Merging Lists {-}

You can merge lists using both `c()` and `list()` functions. Can you tell the difference between the outputs these functions produce? 


```r

a <- list(1, 2, 3)

b <- list (4, 5, 6)

merged_list1 <- c(a, b) 

merged_list1
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] 2
#> 
#> [[3]]
#> [1] 3
#> 
#> [[4]]
#> [1] 4
#> 
#> [[5]]
#> [1] 5
#> 
#> [[6]]
#> [1] 6

merged_list2 <- list(a, b)  

merged_list2
#> [[1]]
#> [[1]][[1]]
#> [1] 1
#> 
#> [[1]][[2]]
#> [1] 2
#> 
#> [[1]][[3]]
#> [1] 3
#> 
#> 
#> [[2]]
#> [[2]][[1]]
#> [1] 4
#> 
#> [[2]][[2]]
#> [1] 5
#> 
#> [[2]][[3]]
#> [1] 6
```

`c()` function merged the elements of `list a` and `list b` and created a list containing 6 elements. In contrast, `list()` function created a list containing two elements, `list a` and `list b`. 


#### Flattening a list into a vector {-}

You can convert a list into a vector using `unlist()` function:



```r

list3 <- list (c(1,2,3), 45, c(20, -5))

unlist(list3)                           
#> [1]  1  2  3 45 20 -5
```


#### Manipulating elements in a list {-}

Adding an element to a list:


```r

list3
#> [[1]]
#> [1] 1 2 3
#> 
#> [[2]]
#> [1] 45
#> 
#> [[3]]
#> [1] 20 -5

list3[4] <- 100   

list3
#> [[1]]
#> [1] 1 2 3
#> 
#> [[2]]
#> [1] 45
#> 
#> [[3]]
#> [1] 20 -5
#> 
#> [[4]]
#> [1] 100
```

Removing an element from a list:


```r

# Removing the second element in the list3

list3[2] <- NULL                           

list3
#> [[1]]
#> [1] 1 2 3
#> 
#> [[2]]
#> [1] 20 -5
#> 
#> [[3]]
#> [1] 100
```


Changing values of elements in a list:



```r

# Changing the second element of the first element of the list3

list3[[1]][3] <- 50                       

list3
#> [[1]]
#> [1]  1  2 50
#> 
#> [[2]]
#> [1] 20 -5
#> 
#> [[3]]
#> [1] 100
```


