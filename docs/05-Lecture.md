# Lecture 05 {-}

&nbsp;


Control structures in R are used to control the flow of execution of various R expressions. They require the programmer to specify one or more conditions to be evaluated or tested by the program, along with a statement or statements to be executed if the condition is determined to be `TRUE`, and optionally, other statements to be executed if the condition is determined to be `FALSE`. In other words, control structures allow you to respond to inputs or to features of the data and execute different R expressions accordingly. 


In this class we will discuss several commonly used control structures such as `if-else` statements, `for` and `while` loops, as well as `break` and `next` commands. Before we move on to these topics, first let's discuss some relational and logical operators.


## Relational and Logical Operations {-}

### Comparison Operators {-}

&nbsp;

The basic relational operators are called comparison operators and are listed in the table below:


Operator | Name | Example
---------|------|--------
==       |Equal | x == y
!=       | Not equal | x != y
>       | Greater than | x > y
<       | Less than | x < y
>=      | Greater than or equal to | x >= y
<=     | Less than or equal to | x <= y


The possible outputs of these operations are either `TRUE` or `FALSE`. Here are some examples with these comparison operators:



``` r

print (5 == 5)
#> [1] TRUE

print(5 == 7)
#> [1] FALSE

print(5 != 5)
#> [1] FALSE

print(5 != 10)
#> [1] TRUE

print(5 < 2)
#> [1] FALSE

print(5 <= 4)
#> [1] FALSE
```


When applied to vectors, these operators do element-wise comparisons (the first element of the first vector is compared to the first element of the second vector and so on). Suppose we have the following two vectors:


``` r

x <- c(1, 2, 3, 4)

print(x)
#> [1] 1 2 3 4

y <- c(4, 2, 3, 1)

print(y)
#> [1] 4 2 3 1
```

Then,


``` r

print(x == y)
#> [1] FALSE  TRUE  TRUE FALSE

print(x != y)
#> [1]  TRUE FALSE FALSE  TRUE

print(x >= y)
#> [1] FALSE  TRUE  TRUE  TRUE
```


### Logical Operators {-}

&nbsp;

Logical operators are used to combine conditional statements:

Operator | Description
---------|-------------
&  | 	Element-wise Logical AND operator. It returns TRUE if both elements are TRUE
$|$   | Element-wise Logical OR operator. It returns TRUE if one of the statement is TRUE
!    | Logical NOT - returns FALSE if statement is TRUE


Here are some examples:


``` r

##  & Operator

print(TRUE & TRUE)
#> [1] TRUE

print (TRUE & FALSE)
#> [1] FALSE

print(FALSE & FALSE)
#> [1] FALSE

print (5 == 5 & 10 < 6)
#> [1] FALSE

print(4!= 3 & 2 < 9)
#> [1] TRUE

print(x == y & x < y)
#> [1] FALSE FALSE FALSE FALSE


##  | Operator

print(TRUE | TRUE)
#> [1] TRUE

print (TRUE | FALSE)
#> [1] TRUE

print(FALSE | FALSE)
#> [1] FALSE

print (5 == 5 | 10 < 6)
#> [1] TRUE

print(4!= 3 | 2 < 9)
#> [1] TRUE

print(x == y | x < y)
#> [1]  TRUE  TRUE  TRUE FALSE


##  ! Operator

print (!TRUE)
#> [1] FALSE

print(!FALSE)
#> [1] TRUE

print(!(4<2))
#> [1] TRUE

print(!(2 == 2 & 3 < 4))
#> [1] FALSE
```


## Conditional Statements {-}

### IF Statement {-}

An "if statement" is written with the `if` keyword, and it is used to specify a block of code to be executed if a condition is `TRUE`:


``` r

if (5 > 2) {
  
  print("5 is greater than 2")
  
}
#> [1] "5 is greater than 2"
```

No code will be executed if the conditional statement inside the parentheses is `FALSE`. Check it yourself:


``` r

if (5 < 2) {
  
  print("5 is greater than 2")
  
}

```


### IF - ELSE Statement {-}

The `if-else` combination is probably the most commonly used control structure in R. This structure allows you to test a condition and act on it depending on whether it’s `TRUE` or `FALSE`. If the condition is `TRUE`, then the code given in curly brackets after the `if` clause will be executed. If it is `FALSE`, then the code in curly brackets after the `else` clause will be executed:


``` r

a <- 5 ; b <- 2


if (b > a) {
  
  print ("b is greater than a")
  
} else {
  
  print("b is not greater than a")
}
#> [1] "b is not greater than a"
```

Now try this example:



``` r

a <- 2 ; b <- 5


if (b > a) {
  
  print ("b is greater than a")
  
} else {
  
  print("b is not greater than a")
}
#> [1] "b is greater than a"
```

You can pass as many conditions as you would like to by adding `else if` clause:


``` r
if (b > a) {
  
  print ("b is greater than a")
  
} else if (a == b) {
  
  print ("a and b are equal")
  
} else {
  
  print ("a is greater than b")
  
}
#> [1] "b is greater than a"
```


Try again with different values:


``` r
a <- 2; b <- 2

if (b > a) {
  
  print ("b is greater than a")
  
} else if (a == b) {
  
  print ("a and b are equal")
  
} else {
  
  print ("a is greater than b")
  
}
#> [1] "a and b are equal"
```


### Nested IF-ELSE Statements {-}

You can also have `if` statements inside of `if` statements, this is called `nested if` statements.



``` r

x <- 50

if (x > 10) {
  
  print("Above ten")
  
  if (x > 20) {
    
    print("and also above 20!")
    
  } else {
    
    print("but not above 20.")
    
  }
  
} else {
  
  print("below 10.")
  
}
#> [1] "Above ten"
#> [1] "and also above 20!"
```

Now try it with `x = 15` and `x = 5`:



``` r

x <- 15

if (x > 10) {
  
  print("Above ten")
  
  if (x > 20) {
    
    print("and also above 20!")
    
  } else {
    
    print("but not above 20.")
    
  }
  
} else {
  
  print("below 10.")
  
}
#> [1] "Above ten"
#> [1] "but not above 20."
```



``` r

x <- 5

if (x > 10) {
  
  print("Above ten")
  
  if (x > 20) {
    
    print("and also above 20!")
    
  } else {
    
    print("but not above 20.")
    
  }
  
} else {
  
  print("below 10.")
  
}
#> [1] "below 10."
```


You can have multiple conditional statements in the `if-else` clause combined together:



``` r

a <- 200; b <- 33; c <- 500


##  &

if (a > b & c == a) {
  
  print("Both conditions are true")
  
} else {
  
  print ("At least one is false")
}
#> [1] "At least one is false"



##  |

if (a > b | c == a) {
  
  print("At least one is true")
  
} else {
  
  print ("Both conditions are false")
}
#> [1] "At least one is true"
```

### ifelse() Function {-}

Given that `if` only works with a single `TRUE` or `FALSE`, you might wonder what to do if you have a vector of logical values. `ifelse()` can handle vectors of logical values. The first argument of `ifelse()` function is a vector of logical values,the second argument is a value that it produces if the element of the logical vector is `TRUE`, and the third argument is a value that it produces if the element of the logical vector is `FALSE`. Check this example out:


``` r
x <- c(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

ifelse(x %% 2 == 0, "even", "odd")
#>  [1] "odd"  "even" "odd"  "even" "odd"  "even" "odd"  "even"
#>  [9] "odd"  "even"
```


### Some Useful Functions: any() and all() {-}

`any()` and `all()` functions come in handy when you work with conditional statement. Here are some examples:


``` r
x <- c(-2, -1, 0, 1, 2)

## all(). It return TRUE if all elements of a vector satisfy the condition

print(all(x > 0))
#> [1] FALSE

## any(). It return TRUE if at least one of elements in a vector satisfies the condition

print(any(x > 0))
#> [1] TRUE


## Here is an example 

if(all(x < 0)) {
  
  print("all are negative")
  
} else {
  
  print ("not all of them are negative")
  
}
#> [1] "not all of them are negative"
```

## For Loops {-}

There may be a situation when you need to execute a block of code multiple times. In general, statements are executed sequentially. The first statement in a function is executed first, followed by the second, and so on.


Programming languages provide various control structures that allow for more complicated execution paths. A loop statement allows us to execute a statement or groups of statements multiple times (**iterate**). It is a sequence of instructions that is repeated until a certain condition is reached. The most basic and commonly used type of loops is a `for` loop. The general format of the `for` loop is as follows:


``` r

for(loop.object in loop.vector) {

  LOOP.CODE

  }

```


There are three key aspects of loops: `loop object`, `loop vector`, and `loop code`.


* `Loop object`: The object that will change for each iteration of the loop. This is usually a letter like `i`.
* `Loop vector`: A vector specifying all values that the loop object will take over the loop. You can specify the values any way you’d like (as long as it’s a vector).
* `Loop code`: The code that will be executed for all values in the loop vector.


Below you can find a few examples on how for loops can be utilized:


``` r

## Printing numbers from 1 to 10

for (i in 1:10){
  
  print (i)
  
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
#> [1] 6
#> [1] 7
#> [1] 8
#> [1] 9
#> [1] 10
```



``` r

## Printing elements of the vector x

x <- c(10, 20, 30, 40, 50)

for (i in 1:length(x)){
  
  print(x[i])
  
}
#> [1] 10
#> [1] 20
#> [1] 30
#> [1] 40
#> [1] 50
```



``` r

## Creating and populating an empty vector z

z <- c()   # creating an empty vector.

for (i in 1:5){
  
  z[i] <- 2*i
  
}

print(z)
#> [1]  2  4  6  8 10
```




``` r

## Creating and populating an empty list

a <- list()   # creating an empty list

for (i in 1:4){
  
  a[[i]] <- rnorm(i)
  
}

print(a)
#> [[1]]
#> [1] -0.5888903
#> 
#> [[2]]
#> [1]  1.8345364 -0.7652574
#> 
#> [[3]]
#> [1] -0.1301552  0.1341216  0.7712844
#> 
#> [[4]]
#> [1]  0.91447576 -1.20602765  0.99811939  0.06846585
```



``` r

## Updating elements of the vector x

x <- c(1, 2, 3, 4, 5)


for (i in 1:length(x)){
  
  x[i] <- x[i] ^ 2
  
}

print(x)
#> [1]  1  4  9 16 25
```


``` r
## Summing up numbers from 1 to 100

current.sum <- 0


for(i in 1:100) {
  
  current.sum <- current.sum + i
  
}

```


### Break Statement {-}

Loop control statements change execution from its normal sequence. `Break` statement stops the execution of a `for` loop when the condition specified is met. In other words, it stops before it has looped through all the items. For example,


``` r

for(i in 1:60) {
  
  if(i > 15){
    
    break
    
  }
  
  print(i)
  
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
#> [1] 6
#> [1] 7
#> [1] 8
#> [1] 9
#> [1] 10
#> [1] 11
#> [1] 12
#> [1] 13
#> [1] 14
#> [1] 15
```

### Next Statement {-}

With the `next` statement, you can skip an undesired iteration without terminating the loop:


``` r
## Skipping even numbers

for(i in 1:100) {
  
  if(i %% 2 == 0){
    
    next
    
  }
  
  print(i)
  
}
#> [1] 1
#> [1] 3
#> [1] 5
#> [1] 7
#> [1] 9
#> [1] 11
#> [1] 13
#> [1] 15
#> [1] 17
#> [1] 19
#> [1] 21
#> [1] 23
#> [1] 25
#> [1] 27
#> [1] 29
#> [1] 31
#> [1] 33
#> [1] 35
#> [1] 37
#> [1] 39
#> [1] 41
#> [1] 43
#> [1] 45
#> [1] 47
#> [1] 49
#> [1] 51
#> [1] 53
#> [1] 55
#> [1] 57
#> [1] 59
#> [1] 61
#> [1] 63
#> [1] 65
#> [1] 67
#> [1] 69
#> [1] 71
#> [1] 73
#> [1] 75
#> [1] 77
#> [1] 79
#> [1] 81
#> [1] 83
#> [1] 85
#> [1] 87
#> [1] 89
#> [1] 91
#> [1] 93
#> [1] 95
#> [1] 97
#> [1] 99
```

### For Loops Combined with If-Else Statements {-}

You can even combine for loops with `if-else` statements:


``` r

x <- 1:10

for (i in 1:length(x)){
  
  if(x[i] %% 2 == 0){
    
    print(paste("The number", x[i], "is even"))
    
  } else {
    
    print(paste("The number", x[i], "is odd"))
    
  }
  
}
#> [1] "The number 1 is odd"
#> [1] "The number 2 is even"
#> [1] "The number 3 is odd"
#> [1] "The number 4 is even"
#> [1] "The number 5 is odd"
#> [1] "The number 6 is even"
#> [1] "The number 7 is odd"
#> [1] "The number 8 is even"
#> [1] "The number 9 is odd"
#> [1] "The number 10 is even"
```

### Nested For Loops {-}

Nested loops are commonly needed for multidimensional or hierarchical data structures (e.g. matrices, lists). Nesting beyond 2 to 3 levels often makes it difficult to read/understand the code. If you find yourself in need of a large number of nested loops, you may want to break up the loops by using functions (discussed in the next lecture). Here is an example of the nested loop:



``` r

## Creating and populating an empty matrix

matrix1 <- matrix(ncol = 4, nrow = 5) # creating an empty matrix


for (i in 1:dim(matrix1)[1]){
  
  for (j in 1:dim(matrix1)[2]){
    
    matrix1[i, j] <- i*j
    
  }
}

print(matrix1)
#>      [,1] [,2] [,3] [,4]
#> [1,]    1    2    3    4
#> [2,]    2    4    6    8
#> [3,]    3    6    9   12
#> [4,]    4    8   12   16
#> [5,]    5   10   15   20
```


## While Loops {-}

`While` loops begin by testing a condition. If it is `TRUE`, then they execute the loop body. Once the loop body is executed, the condition is tested again, and so forth, until the condition is `FALSE`, after which the loop exits.


``` r

i <- 1

while (i < 10){
  
  print(i)
  
  i <- i + 1
  
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
#> [1] 6
#> [1] 7
#> [1] 8
#> [1] 9
```

### While Loops with Next Statements {-}

While loops can be combined with `next` statements:


``` r

# Printing odd numbers that are less than 15

i <- 0

while (i < 15){
  
  i <- i + 1
  
  if(i %% 2 == 0){
    
    next
  }
  
  print(i)
  
}
#> [1] 1
#> [1] 3
#> [1] 5
#> [1] 7
#> [1] 9
#> [1] 11
#> [1] 13
#> [1] 15
```


While loops can potentially result in infinite loops if not written properly. **Use with care!** If you accidentally created and executed infinite loops, it will run forever and take up all your storage in R. To stop an infinite while loop, press the red stop sign that shows up in the top right corner of the console. Can you check and tell what is wrong with the while loops given below:


``` r

i <- 1

while (i < 15){
  
  if(i %% 2 == 0){
    
    next
  }

  print(i)
    
  i <- i + 1
  
}

```



``` r

i <- 1

while (i < 15){
  
  print(i)
  
  if(i %% 2 == 0){
    
    next
  }
  
  i <- i + 1
  
}

```


``` r

i <- 1

while (i < 15){
  
  print(i)
  
  i <- i + 1
  
  if(i %% 2 == 0){
    
    next
  }
  
}

```

### While Loops with Break Statements {-}

Similarly, while loops can be combined with `break` statements:


``` r

i <- 1

while (i < 20){
  
  print (i)
  
  if (i == 5){
    
    break
    
  }
  
  i <- i + 1
  
  
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
```


Can you tell what would be the output of these while loops without running them?



``` r

i <- 1

while (i < 20){
  
  print (i)
  
  if (i == 5){
    
    break
    
  }
  
  i <- i + 1
  
  
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
```


``` r

i <- 1

while (i < 20){
  
  if (i == 5){
    
    break
    
  }
  
  print (i)
  
  i <- i + 1
  
}
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
```
