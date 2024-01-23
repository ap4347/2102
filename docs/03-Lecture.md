# Lecture 03 {-}

&nbsp;

We continue exploring different data structures in R programming language. In this lecture we will discuss `matrices`, `data frames`, and `factors`.


## Matrices {-}


Matrices are R objects in which elements are arranged in a two-dimensional rectangular layout (columns and rows). A column is a vertical representation of data, while a row is a horizontal representation of data. Like vectors, matrices work with homogeneous data only.

### Creating Matrices {-}

We use `matrix()` function to create a matrix. It has the following arguments (input values):

`matrix(data, nrow, ncol, byrow, dimnames)`

* **data** is the input vector which becomes the data elements of the matrix
* **nrow** is the number of rows to be created 
* **ncol** is the number of columns to be created
* **byrow** is a logical argument. If `TRUE` then the input vector elements are arranged by row
* **dimname** is the names assigned to the rows and columns

&nbsp;

let's create a matrix containing `c(1:12)` elements:


```r

# Elements are arranged sequentially by row

matrix_1 <- matrix(c(1:12), nrow = 4, byrow = TRUE)

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12


# Elements are arranged sequentially by column

matrix_2 <- matrix(c(1:12), nrow = 4, byrow = FALSE)

print(matrix_2)
#>      [,1] [,2] [,3]
#> [1,]    1    5    9
#> [2,]    2    6   10
#> [3,]    3    7   11
#> [4,]    4    8   12


# Specifying the number of columns instead

matrix_3 <- matrix(c(1:12), ncol = 4, byrow = TRUE)

print(matrix_3)
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3    4
#> [2,]    5    6    7    8
#> [3,]    9   10   11   12


# Defining names of columns and rows in a matrix

rows <- c("row1", "row2", "row3", "row4")

cols <- c("col1", "col2", "col3")

matrix_4 <- matrix(c(1:12), nrow = 4, byrow = TRUE, dimnames = list(rows, cols))

print(matrix_4)
#>      col1 col2 col3
#> row1    1    2    3
#> row2    4    5    6
#> row3    7    8    9
#> row4   10   11   12
```


### Accessing Matrix Elements {-}

Elements of a matrix can be accessed by using the column and row indexes of the element and square brackets:



```r

# Accessing the element in the 3rd column and 1st row

matrix_1[1, 3]
#> [1] 3

# Accessing the element in the 2nd column and 4th row

matrix_1[4, 2]
#> [1] 11

# Accessing only the 2nd row

matrix_1[2, ]
#> [1] 4 5 6

# Accessing only the 3rd column

matrix_1[, 3]
#> [1]  3  6  9 12

# Accessing only the 2nd and 3rd rows 

matrix_1[c(2,3), ]
#>      [,1] [,2] [,3]
#> [1,]    4    5    6
#> [2,]    7    8    9
```


### Adding and Removing Rows & Columns {-}

Use `cbind()` function to add additional columns into a matrix:


```r

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12

matrix_5 <- cbind(matrix_1, c(10, 20, 30, 40))

print(matrix_5)
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3   10
#> [2,]    4    5    6   20
#> [3,]    7    8    9   30
#> [4,]   10   11   12   40
```


Use `rbind()` function to add additional rows into a matrix:


```r

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12

matrix_6 <- rbind(matrix_1, c(10, 20, 30))

print(matrix_6)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12
#> [5,]   10   20   30
```


Use the `c()` function along with negative indexes to remove rows and columns from the matrix:


```r

# Removing the 1st and 2nd rows and 1st column from a matrix

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12

matrix_7 <- matrix_1[c(-1, -2), c(-1)]

print(matrix_7)
#>      [,1] [,2]
#> [1,]    8    9
#> [2,]   11   12
```


### Assigning Values to Matrix Elements {-}


```r

# Assigning a single value

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12

matrix_1[1, 1] <- 100

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]  100    2    3
#> [2,]    4    5    6
#> [3,]    7    8    9
#> [4,]   10   11   12

# Assigning a row

matrix_1[2, ] <- c(11, 22, 33)

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]  100    2    3
#> [2,]   11   22   33
#> [3,]    7    8    9
#> [4,]   10   11   12

# Replace elements that are equal to 8 with 0

matrix_1[matrix_1 == 8] <- 0

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]  100    2    3
#> [2,]   11   22   33
#> [3,]    7    0    9
#> [4,]   10   11   12
```


### Matrix Operations {-}


```r

matrix_8 <- matrix(1:8, nrow = 4, byrow = T)

print(matrix_8)
#>      [,1] [,2]
#> [1,]    1    2
#> [2,]    3    4
#> [3,]    5    6
#> [4,]    7    8

matrix_9 <- matrix(1:8, nrow = 4, byrow = T)

print(matrix_9)
#>      [,1] [,2]
#> [1,]    1    2
#> [2,]    3    4
#> [3,]    5    6
#> [4,]    7    8

# Elementwise Addition

print(matrix_8 + matrix_9)
#>      [,1] [,2]
#> [1,]    2    4
#> [2,]    6    8
#> [3,]   10   12
#> [4,]   14   16

# Elementwise Subtraction

print(matrix_8 - matrix_9)
#>      [,1] [,2]
#> [1,]    0    0
#> [2,]    0    0
#> [3,]    0    0
#> [4,]    0    0

# Elementwise Multiplication

print(matrix_8 * matrix_9)
#>      [,1] [,2]
#> [1,]    1    4
#> [2,]    9   16
#> [3,]   25   36
#> [4,]   49   64

# Elementwise Division

print(matrix_8 / matrix_9)
#>      [,1] [,2]
#> [1,]    1    1
#> [2,]    1    1
#> [3,]    1    1
#> [4,]    1    1

# Multiplication by Constant

print(2 * matrix_8)
#>      [,1] [,2]
#> [1,]    2    4
#> [2,]    6    8
#> [3,]   10   12
#> [4,]   14   16

# Matrix Multiplication

matrix_10 <- matrix(1:8, nrow = 2, byrow = T)

print(matrix_10)
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3    4
#> [2,]    5    6    7    8

print(matrix_8 %*% matrix_10)
#>      [,1] [,2] [,3] [,4]
#> [1,]   11   14   17   20
#> [2,]   23   30   37   44
#> [3,]   35   46   57   68
#> [4,]   47   62   77   92

# Matrix Transpose

print(t(matrix_8))
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    3    5    7
#> [2,]    2    4    6    8
```


### Matrix Dimensions {-}

Use `dim()` function to check matrix dimensions:


```r

print(matrix_1)
#>      [,1] [,2] [,3]
#> [1,]  100    2    3
#> [2,]   11   22   33
#> [3,]    7    0    9
#> [4,]   10   11   12

dim(matrix_1)
#> [1] 4 3
```

`length()` function, when applied to a matrix, shows how many elements are stored in the matrix:


```r

length(matrix_1)
#> [1] 12
```


## Data Frames {-}

A data frame is a table in which each column contains values of one variable and each row contains one set of values from each column. In other words, it is a data structure that represents cases in which there are observations (rows) and measurements (columns). Following are the characteristics of a data frame:

* The column names should be non-empty
* The data stored in a data frame can be of numeric, factor (discussed later), or character type
* Each column should contain same number of data items

### Creating a Data Frame {-}

We can use a `data.frame()` function to create a data frame from a set of vectors:



```r

Name <- c("James", "Linda", "Stacy", "Mary", "Tom", "Anna", "Bob", "Jeniffer", "Lucas", "Amy")

Age <- c(22, 56, 34, 48, 19, 31, 68, 72, 39, 52)

State <- c("California", "New York", "New York", "Michigan", "Texas", "Ohio", "Arizona", "Florida", "Nebraska", "Indiana")

Salary <- c(30000, 96500, 72000, 54300, 25000, 61000, 74700, 40000, 83000, 92400)

df <- data.frame(Name, Age, State, Salary)

print(df)
#>        Name Age      State Salary
#> 1     James  22 California  30000
#> 2     Linda  56   New York  96500
#> 3     Stacy  34   New York  72000
#> 4      Mary  48   Michigan  54300
#> 5       Tom  19      Texas  25000
#> 6      Anna  31       Ohio  61000
#> 7       Bob  68    Arizona  74700
#> 8  Jeniffer  72    Florida  40000
#> 9     Lucas  39   Nebraska  83000
#> 10      Amy  52    Indiana  92400
```

We can use `srt()` function to check a structure of the data frame and `summary()` to get a quick summary on the variables stored in this data frame:


```r

# Data frame structure

str(df)
#> 'data.frame':	10 obs. of  4 variables:
#>  $ Name  : chr  "James" "Linda" "Stacy" "Mary" ...
#>  $ Age   : num  22 56 34 48 19 31 68 72 39 52
#>  $ State : chr  "California" "New York" "New York" "Michigan" ...
#>  $ Salary: num  30000 96500 72000 54300 25000 61000 74700 40000 83000 92400

# Summary of variables stored in a data frame

summary(df)
#>      Name                Age           State          
#>  Length:10          Min.   :19.00   Length:10         
#>  Class :character   1st Qu.:31.75   Class :character  
#>  Mode  :character   Median :43.50   Mode  :character  
#>                     Mean   :44.10                     
#>                     3rd Qu.:55.00                     
#>                     Max.   :72.00                     
#>      Salary     
#>  Min.   :25000  
#>  1st Qu.:43575  
#>  Median :66500  
#>  Mean   :62890  
#>  3rd Qu.:80925  
#>  Max.   :96500
```


`head()` and `tail()` functions allow us to view the first and last 6 rows of a data frame, respectively (number of rows can be specified as an argument):



```r

head(df)
#>    Name Age      State Salary
#> 1 James  22 California  30000
#> 2 Linda  56   New York  96500
#> 3 Stacy  34   New York  72000
#> 4  Mary  48   Michigan  54300
#> 5   Tom  19      Texas  25000
#> 6  Anna  31       Ohio  61000

tail(df)
#>        Name Age    State Salary
#> 5       Tom  19    Texas  25000
#> 6      Anna  31     Ohio  61000
#> 7       Bob  68  Arizona  74700
#> 8  Jeniffer  72  Florida  40000
#> 9     Lucas  39 Nebraska  83000
#> 10      Amy  52  Indiana  92400

head(df, 3)
#>    Name Age      State Salary
#> 1 James  22 California  30000
#> 2 Linda  56   New York  96500
#> 3 Stacy  34   New York  72000
```


Use `colnames()` function to get a list of column names or to change the names of columns in a data frame:



```r

colnames(df)
#> [1] "Name"   "Age"    "State"  "Salary"

colnames(df)[1] <- "NAME"

print(df)
#>        NAME Age      State Salary
#> 1     James  22 California  30000
#> 2     Linda  56   New York  96500
#> 3     Stacy  34   New York  72000
#> 4      Mary  48   Michigan  54300
#> 5       Tom  19      Texas  25000
#> 6      Anna  31       Ohio  61000
#> 7       Bob  68    Arizona  74700
#> 8  Jeniffer  72    Florida  40000
#> 9     Lucas  39   Nebraska  83000
#> 10      Amy  52    Indiana  92400

colnames(df) <- c("NAME", "AGE", "STATE", "SALARY")

print(df)
#>        NAME AGE      STATE SALARY
#> 1     James  22 California  30000
#> 2     Linda  56   New York  96500
#> 3     Stacy  34   New York  72000
#> 4      Mary  48   Michigan  54300
#> 5       Tom  19      Texas  25000
#> 6      Anna  31       Ohio  61000
#> 7       Bob  68    Arizona  74700
#> 8  Jeniffer  72    Florida  40000
#> 9     Lucas  39   Nebraska  83000
#> 10      Amy  52    Indiana  92400
```



### Subsetting/Extracting the Elements/Columns/Rows from a Data Frame {-}

We can use `[]`, `$`, `[[]]` to extract elements, columns, or rows from a data frame (uses the same rule as matrices):


```r

# Extracting the first column using its position; output: data frame

df[1]
#>        NAME
#> 1     James
#> 2     Linda
#> 3     Stacy
#> 4      Mary
#> 5       Tom
#> 6      Anna
#> 7       Bob
#> 8  Jeniffer
#> 9     Lucas
#> 10      Amy

# Extracting the first column using its position; output: vector

df[[1]]
#>  [1] "James"    "Linda"    "Stacy"    "Mary"     "Tom"     
#>  [6] "Anna"     "Bob"      "Jeniffer" "Lucas"    "Amy"

# Extracting a column using its name; output: data frame

df["AGE"]
#>    AGE
#> 1   22
#> 2   56
#> 3   34
#> 4   48
#> 5   19
#> 6   31
#> 7   68
#> 8   72
#> 9   39
#> 10  52

df[c("AGE", "STATE")]
#>    AGE      STATE
#> 1   22 California
#> 2   56   New York
#> 3   34   New York
#> 4   48   Michigan
#> 5   19      Texas
#> 6   31       Ohio
#> 7   68    Arizona
#> 8   72    Florida
#> 9   39   Nebraska
#> 10  52    Indiana

# Extracting a column using its name; output: vector

df[["AGE"]]
#>  [1] 22 56 34 48 19 31 68 72 39 52

# Extracting a column using its name and '$' operator; output: vector

df$AGE
#>  [1] 22 56 34 48 19 31 68 72 39 52

# Extracting a single element

df[1, 2]
#> [1] 22

# Extracting multiple elements

df[1:4, c(1, 3)]
#>    NAME      STATE
#> 1 James California
#> 2 Linda   New York
#> 3 Stacy   New York
#> 4  Mary   Michigan

# Extracting the first row only

df[1, ]
#>    NAME AGE      STATE SALARY
#> 1 James  22 California  30000

# Extracting the third column only

df[, 3]
#>  [1] "California" "New York"   "New York"   "Michigan"  
#>  [5] "Texas"      "Ohio"       "Arizona"    "Florida"   
#>  [9] "Nebraska"   "Indiana"

# Extracting all elements except the second column

df[, -2]
#>        NAME      STATE SALARY
#> 1     James California  30000
#> 2     Linda   New York  96500
#> 3     Stacy   New York  72000
#> 4      Mary   Michigan  54300
#> 5       Tom      Texas  25000
#> 6      Anna       Ohio  61000
#> 7       Bob    Arizona  74700
#> 8  Jeniffer    Florida  40000
#> 9     Lucas   Nebraska  83000
#> 10      Amy    Indiana  92400

# Extracting elements using logical operator

df[df$AGE < 50, ]
#>    NAME AGE      STATE SALARY
#> 1 James  22 California  30000
#> 3 Stacy  34   New York  72000
#> 4  Mary  48   Michigan  54300
#> 5   Tom  19      Texas  25000
#> 6  Anna  31       Ohio  61000
#> 9 Lucas  39   Nebraska  83000
```


### Assigning New Value to Data Frame Elements {-}

Like in matrices, we can use `<-` operator to assign values:


```r

df[1, 4] <- 100000

print(df)
#>        NAME AGE      STATE SALARY
#> 1     James  22 California 100000
#> 2     Linda  56   New York  96500
#> 3     Stacy  34   New York  72000
#> 4      Mary  48   Michigan  54300
#> 5       Tom  19      Texas  25000
#> 6      Anna  31       Ohio  61000
#> 7       Bob  68    Arizona  74700
#> 8  Jeniffer  72    Florida  40000
#> 9     Lucas  39   Nebraska  83000
#> 10      Amy  52    Indiana  92400
```


### Adding/Removing Columns/Rows {-}




```r

# Adding a new column using `$` operator

df$EXPERIENCE = c(2, 30, 10, 22, 1, 12, 40, 55, 15, 22)

print(df)
#>        NAME AGE      STATE SALARY EXPERIENCE
#> 1     James  22 California 100000          2
#> 2     Linda  56   New York  96500         30
#> 3     Stacy  34   New York  72000         10
#> 4      Mary  48   Michigan  54300         22
#> 5       Tom  19      Texas  25000          1
#> 6      Anna  31       Ohio  61000         12
#> 7       Bob  68    Arizona  74700         40
#> 8  Jeniffer  72    Florida  40000         55
#> 9     Lucas  39   Nebraska  83000         15
#> 10      Amy  52    Indiana  92400         22

# Adding a new column using `cbind()` function

Sex <- c("Male", "Female", "Female", "Female", "Male", "Female", "Male", "Female", "Male", "Female")

df_2 <- cbind(df, Sex)

print(df_2)
#>        NAME AGE      STATE SALARY EXPERIENCE    Sex
#> 1     James  22 California 100000          2   Male
#> 2     Linda  56   New York  96500         30 Female
#> 3     Stacy  34   New York  72000         10 Female
#> 4      Mary  48   Michigan  54300         22 Female
#> 5       Tom  19      Texas  25000          1   Male
#> 6      Anna  31       Ohio  61000         12 Female
#> 7       Bob  68    Arizona  74700         40   Male
#> 8  Jeniffer  72    Florida  40000         55 Female
#> 9     Lucas  39   Nebraska  83000         15   Male
#> 10      Amy  52    Indiana  92400         22 Female

# Adding a new row using `rbind()` function

new_obs <- data.frame(NAME = "Jack", AGE = 41, STATE = "Texas", SALARY = 150000, EXPERIENCE = 20)

df_3 <- rbind(df, new_obs)

print(df_3)
#>        NAME AGE      STATE SALARY EXPERIENCE
#> 1     James  22 California 100000          2
#> 2     Linda  56   New York  96500         30
#> 3     Stacy  34   New York  72000         10
#> 4      Mary  48   Michigan  54300         22
#> 5       Tom  19      Texas  25000          1
#> 6      Anna  31       Ohio  61000         12
#> 7       Bob  68    Arizona  74700         40
#> 8  Jeniffer  72    Florida  40000         55
#> 9     Lucas  39   Nebraska  83000         15
#> 10      Amy  52    Indiana  92400         22
#> 11     Jack  41      Texas 150000         20

# Removing columns from a data frame using the `c()` function and negative indexes

df_4 <- df_3[, -4]

print(df_4)
#>        NAME AGE      STATE EXPERIENCE
#> 1     James  22 California          2
#> 2     Linda  56   New York         30
#> 3     Stacy  34   New York         10
#> 4      Mary  48   Michigan         22
#> 5       Tom  19      Texas          1
#> 6      Anna  31       Ohio         12
#> 7       Bob  68    Arizona         40
#> 8  Jeniffer  72    Florida         55
#> 9     Lucas  39   Nebraska         15
#> 10      Amy  52    Indiana         22
#> 11     Jack  41      Texas         20

# Removing columns by assigning a `NULL` value

df_3[c("AGE", "SALARY")] <- NULL

print(df_3)
#>        NAME      STATE EXPERIENCE
#> 1     James California          2
#> 2     Linda   New York         30
#> 3     Stacy   New York         10
#> 4      Mary   Michigan         22
#> 5       Tom      Texas          1
#> 6      Anna       Ohio         12
#> 7       Bob    Arizona         40
#> 8  Jeniffer    Florida         55
#> 9     Lucas   Nebraska         15
#> 10      Amy    Indiana         22
#> 11     Jack      Texas         20

# Removing rows from a data frame

df_3[-11, ]
#>        NAME      STATE EXPERIENCE
#> 1     James California          2
#> 2     Linda   New York         30
#> 3     Stacy   New York         10
#> 4      Mary   Michigan         22
#> 5       Tom      Texas          1
#> 6      Anna       Ohio         12
#> 7       Bob    Arizona         40
#> 8  Jeniffer    Florida         55
#> 9     Lucas   Nebraska         15
#> 10      Amy    Indiana         22
```


### Re-ordering Columns/Rows in a Data Frame {-}

In order to re-order columns in a data frame, pass a vector with the desired order of columns using `[]` operator:


```r

# Re-ordering columns in a data frame

df_3[, c("EXPERIENCE", "NAME", "STATE")]
#>    EXPERIENCE     NAME      STATE
#> 1           2    James California
#> 2          30    Linda   New York
#> 3          10    Stacy   New York
#> 4          22     Mary   Michigan
#> 5           1      Tom      Texas
#> 6          12     Anna       Ohio
#> 7          40      Bob    Arizona
#> 8          55 Jeniffer    Florida
#> 9          15    Lucas   Nebraska
#> 10         22      Amy    Indiana
#> 11         20     Jack      Texas

# Re-ordering rows in a data frame

df_3[order(df_3$EXPERIENCE), ]     # Ascending order
#>        NAME      STATE EXPERIENCE
#> 5       Tom      Texas          1
#> 1     James California          2
#> 3     Stacy   New York         10
#> 6      Anna       Ohio         12
#> 9     Lucas   Nebraska         15
#> 11     Jack      Texas         20
#> 4      Mary   Michigan         22
#> 10      Amy    Indiana         22
#> 2     Linda   New York         30
#> 7       Bob    Arizona         40
#> 8  Jeniffer    Florida         55

df_3[order(df_3$EXPERIENCE, decreasing = TRUE), ] # Descending order
#>        NAME      STATE EXPERIENCE
#> 8  Jeniffer    Florida         55
#> 7       Bob    Arizona         40
#> 2     Linda   New York         30
#> 4      Mary   Michigan         22
#> 10      Amy    Indiana         22
#> 11     Jack      Texas         20
#> 9     Lucas   Nebraska         15
#> 6      Anna       Ohio         12
#> 3     Stacy   New York         10
#> 1     James California          2
#> 5       Tom      Texas          1
```


### Checking Dimensions of a Data Frame {-}


```r

# Data frame dimensions

dim(df)
#> [1] 10  5

# Number of columns in a data frame

ncol(df)
#> [1] 5

# Number of rows in a data frame

nrow(df)
#> [1] 10
```




## Factors {-}

Factor is a special type of vectors used to categorize data and store it as levels. It can store both character and integer types of data. They are useful in the columns which have a limited number of unique values. For instance, `Female` or `Male`, `TRUE` or `FALSE` etc. 

### Creating Factors {-}

Use `factor()` function in R to create a factor:


```r

vec1 <- c("YES", "NO", "YES", "YES", "YES", "NO", "NO", "YES")

print(vec1)
#> [1] "YES" "NO"  "YES" "YES" "YES" "NO"  "NO"  "YES"

is.factor(vec1)
#> [1] FALSE

fac1 <- factor(vec1)

print(fac1)
#> [1] YES NO  YES YES YES NO  NO  YES
#> Levels: NO YES

is.factor(fac1)
#> [1] TRUE


#############


vec2 <- c("Jazz", "Rock", "Classic", "Pop", "Classic", "Jazz", "Jazz", "Rock")

fac2 <- factor(vec2)

print(fac2)
#> [1] Jazz    Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock


#############


vec3 <- c(1, 2, 3, 2, 2, 3, 1, 3, 2, 3, 1, 1)

fac3 <- factor(vec3)

print(fac3)
#>  [1] 1 2 3 2 2 3 1 3 2 3 1 1
#> Levels: 1 2 3
```

To create an ordered factor, use `order` argument inside the `factor()` function:


```r

fac4 <- factor(vec3, ordered = TRUE)

print(fac4)
#>  [1] 1 2 3 2 2 3 1 3 2 3 1 1
#> Levels: 1 < 2 < 3
```

You can convert a numeric variable into a factor by dividing it into intervals (segments) using a `cut()` function:


```r

age_factored <- cut(df_4$AGE, 3)

print(age_factored)
#>  [1] (18.9,36.7] (54.3,72.1] (18.9,36.7] (36.7,54.3]
#>  [5] (18.9,36.7] (18.9,36.7] (54.3,72.1] (54.3,72.1]
#>  [9] (36.7,54.3] (36.7,54.3] (36.7,54.3]
#> Levels: (18.9,36.7] (36.7,54.3] (54.3,72.1]
```

You can even add labels for these groups:


```r

age_factored <- cut(df_4$AGE, 3, labels = c("Group1", "Group2", "Group3"))

print(age_factored)
#>  [1] Group1 Group3 Group1 Group2 Group1 Group1 Group3 Group3
#>  [9] Group2 Group2 Group2
#> Levels: Group1 Group2 Group3
```

### Factor Levels {-}

Use `levels()` function to see what levels a factor variable has:


```r

levels(fac1)
#> [1] "NO"  "YES"

levels(fac2)
#> [1] "Classic" "Jazz"    "Pop"     "Rock"

levels(fac3)
#> [1] "1" "2" "3"

levels(fac4)
#> [1] "1" "2" "3"
```


You can also set the levels by adding the `levels` argument inside the `factor()` function:


```r


vec2 <- c("Jazz", "Rock", "Classic", "Pop", "Classic", "Jazz", "Jazz", "Rock")

fac5 <- factor(vec2, levels = c("Classic", "Jazz", "Pop", "Rock", "Other"))

print(fac5)
#> [1] Jazz    Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock Other
```


You can change the order of levels by passing the `level` argument to a `factor` function with the desired order of levels:


```r


print(fac3)
#>  [1] 1 2 3 2 2 3 1 3 2 3 1 1
#> Levels: 1 2 3

fac6 <- factor(vec3, levels = c(3, 1, 2))

print(fac6)
#>  [1] 1 2 3 2 2 3 1 3 2 3 1 1
#> Levels: 3 1 2
```


### Accessing Factor Elements {-}

You can access elements stored in a factor the same way as it was done with vectors, using `[]`.

### Assigning/Changing Values to/of Factor Elements {-}

You can assign a new value to factor elements using the assignment sign `<-`:



```r

print(fac2)
#> [1] Jazz    Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock

fac2[1] <- "Classic"

print(fac2)
#> [1] Classic Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock
```


But what happens when  you assign a new value that  is not already specified in the factor levels?


```r

print(fac2)
#> [1] Classic Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock

fac2[1] <- "Opera"
#> Warning in `[<-.factor`(`*tmp*`, 1, value = "Opera"):
#> invalid factor level, NA generated

print(fac2)
#> [1] <NA>    Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock
```

It will generate `NA`, because there is no `Opera` option among the levels specified in the variable. To add this value, we first need to add it to the levels:


```r

fac7 <- factor(fac2, levels = c("Classic", "Jazz", "Pop", "Rock", "Other", "Opera"))

fac7[1] <- "Opera"

print(fac7)
#> [1] Opera   Rock    Classic Pop     Classic Jazz    Jazz   
#> [8] Rock   
#> Levels: Classic Jazz Pop Rock Other Opera
```


### Generating Factors {-}

We can generate factor levels using the `gl()` function. It takes two integers as input values which indicate how many levels there are and how many times each level is being used.


```r

fac8 <- gl(4, 5, labels = c("Fall", "Winter", "Spring", "Summer"))

print(fac8)
#>  [1] Fall   Fall   Fall   Fall   Fall   Winter Winter Winter
#>  [9] Winter Winter Spring Spring Spring Spring Spring Summer
#> [17] Summer Summer Summer Summer
#> Levels: Fall Winter Spring Summer
```


### Counting Factor Elements/Frequency of Levels {-}



```r

# Calculating a number of elements stored in the factor

length(fac2)
#> [1] 8

# Displaying a frequency of each level of the factor

table(fac2)
#> fac2
#> Classic    Jazz     Pop    Rock 
#>       2       2       1       2
```


### Converting Numeric Data into Factors and Back {-}


```r

# Converting numeric data into factors using as.factor() function

vec3 <- c(0, 1, 2, 2, 1, 0, 2, 1, 0)

fac9 <- as.factor(vec3)

print(fac9)
#> [1] 0 1 2 2 1 0 2 1 0
#> Levels: 0 1 2

# Converting factors back into numeric data using as.numeric() function (part I)

as.numeric(fac9)
#> [1] 1 2 3 3 2 1 3 2 1

# Converting factors back into numeric data using as.numeric() function (part II)

as.numeric(levels(fac9)[fac9])
#> [1] 0 1 2 2 1 0 2 1 0
```
