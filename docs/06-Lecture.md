# Lecture 06 {-}

&nbsp;

## Functions {-}


Now, it’s time to discuss one of the most essential concepts of R programming, that is, `R functions`. Writing functions is a core activity of an R programmer. Functions are often used to encapsulate a sequence of expressions that need to be executed numerous times, perhaps under slightly different conditions. In other words, functions allow us to automate common tasks in a more powerful and general way than simply copy-and-pasting. They are also often written when code must be shared with others or the public.


You should consider writing a function whenever you’ve copied and pasted a block of code more than twice. In this class, we will learn how to create a customized (user-defined) functions and will discuss essential tools that `R functions` are equipped with. 

### Creating a Function {-}

A function is a set of statements organized together to perform a specific task. R has a large number of in-built functions and the user can create their own functions as well. In R, a function is an object so the R interpreter is able to pass control to the function, along with arguments that may be necessary for the function to accomplish the actions.


An `R function` is created by using the `function()` function (Yeah, too many functions). The basic syntax of an R function is as follows:



``` r

function_name <- function(arg_1, arg_2, ...) {
  
   Function body 
}

```

The main components of an R function are:

* `Function Name` - this is the name of your function. It is stored in R environment as an object under this name.
* `Arguments` - an argument is a placeholder. When a function is invoked, you pass a value to the argument. Arguments are optional; that is, a function may contain no arguments. Also arguments can have default values. In this example, arguments are `arg_1`, `arg_2` etc.
* `Function Body` − the function body contains a collection of statements that defines what the function does.
* `Return Value` − the return value of a function is the last expression in the function body to be evaluated.


Here is an example of a simple function that prints `"Hello class 2102!"`:


``` r

first_function <- function() { 
  
  print("Hello class 2102!")
  
}

```

To call a function , use the function name followed by parenthesis:


``` r

first_function()
#> [1] "Hello class 2102!"
```


Here is another example of a simple function:


``` r

new_function <- function(){
  
  for(i in 1:5) {
    
    print(i^2)
    
  }
  
}

```



``` r

new_function()
#> [1] 1
#> [1] 4
#> [1] 9
#> [1] 16
#> [1] 25
```


To check the collection of statements that a function contains, use the function name without parenthesis:


``` r

new_function
#> function () 
#> {
#>     for (i in 1:5) {
#>         print(i^2)
#>     }
#> }
#> <bytecode: 0x000001f65744d3c0>
```


### Function Arguments {-}

Information can be passed into functions as arguments. Arguments are specified after the function name, inside the parentheses. You can add as many arguments as you want, just separate them with a comma. Here is an example of a function with a single argument:



``` r

fun_1 <- function(x){
  
  return(x^2)
  
}

```



``` r

fun_1(x = 5)
#> [1] 25
```


Now let's create a function with two arguments:


``` r

fun_2 <- function(x, y){
  
  return(x - y)
  
}

```



``` r

fun_2(x = 5, y = 7)
#> [1] -2
```


#### Argument Matching {-}

Calling an R function with arguments can be done in a variety of ways. This may be confusing at first, but it’s really handing when doing interactive work at the command line. R functions arguments can be matched by name or by position.


When matched by position, R assigns the first value to the first argument, the second value to second argument and so on. For example, `fun_2(5, 7)` means R will assign `5` to the `x` argument and `7` to `y`.


``` r

fun_2(5, 7)
#> [1] -2
```

Now try it other way around:


``` r

fun_2(7, 5)
#> [1] 2
```


When specifying the function arguments by name, it doesn’t matter in what order you specify them:



``` r

fun_2(x = 5, y = 7)
#> [1] -2
```


``` r

fun_2(y = 7, x = 5)
#> [1] -2
```


#### Number of Arguments {-}

By default, a function must be called with the correct number of arguments. Meaning that if your function expects 2 arguments, you have to call the function with 2 arguments, not more, and not less. Try these examples out and see what you get:


``` r

fun_2(7)
#> Error in fun_2(7): argument "y" is missing, with no default
```



``` r

fun_2(7, 5, 3)
#> Error in fun_2(7, 5, 3): unused argument (3)
```


#### Arguments with Default Values {-}

You can assign default values to arguments in a function. If you call the function without an argument, it uses the default value:



``` r

fun_3 <- function(x = 2, y){
  
  return (x - y)
  
}

```



``` r

fun_3(y = 4)
#> [1] -2
```

But you can still pass new values to arguments with default settings:


``` r

fun_3(x = 10, y = 3)
#> [1] 7
```


***

R objects with various data structures can be passed to a function as arguments. For instance, we pass vectors `vec1` and `vec2` to the function `fun_4`:


``` r

vec1 <- c(1, 2, 3, 4, 5)

vec2 <- 1:5

fun_4 <- function(x, y){
  
  return(x + y)
  
}

```



``` r

fun_4(x = vec1, y = vec2)
#> [1]  2  4  6  8 10
```

&nbsp;

### Lazy Evaluation {-}

Arguments to functions are evaluated **lazily**, so they are evaluated only as needed in the body of the function. For example, the function `fun_5` has two arguments, but it uses only one of them (argument `x`).


``` r

fun_5 <- function(x, y){
  
  return(x + 2)
}

```

So calling `fun_5(x = 5)` will not produce an error, because the missing argument `y` is not used inside the body of the function. 


``` r

fun_5(x = 3)
#> [1] 5
```


But if we omit argument `x` while calling the function `fun_5`, it will produce an error, because the body of the function uses `x` argument to produce the output:


``` r

fun_5(y = 3)
#> Error in fun_5(y = 3): argument "x" is missing, with no default
```

&nbsp;

### Function Environments and Global Variables {-}

When R tries to bind a value to a symbol, it searches through a series of **environments** to find the appropriate value. An environment is a collection of (symbol, value) pairs, i.e. `x` is a symbol and `3.14` might be its value. In this class, we will consider two environments: **Global Environment** and **Functional Environment**.


The _global environment_ or the user's workspace stores all R objects that you create while working in R. In contrast to global environment, the _functional environment_ contains all objects that are created inside of a function.


When you call an R function, it first searches for objects in the function's environment and then in the global environment. For instance, compare the functions given below. What would be the output in each case?



``` r

a <- 5                        # this variable exists in the global environment

fun_6 <- function(x){
  
  return(x + a)
  
}


fun_6(x  = 2)
#> [1] 7
```



``` r

a <- 5                        # this variable exists in the global environment

fun_7 <- function(x){
  
  a <- 10                    # this variable exists in the environment of fun_7
  
  return(x + a)
  
}


fun_7(x = 2)
#> [1] 12
```

In another example given below, a variable `b` exists in the environment of `func_8` only. So, if you try to print out `b` object separately, R will throw an error, because such an object does not exist in the global environment:


``` r

fun_8 <- function(x){
  
  a <- 10                   # this variable exists in the environment of fun_8
  
  b <- 20                   # this variable exists in the environment of fun_8
  
  return(x + a + b)
  
}

```



``` r

print(b)
#> Error: object 'b' not found
```


Now, if you want to create an object inside of a function that will be accessibly in global environment as well, then you should use `<<-` operator:


``` r

fun_8 <- function(x){
  
  a <- 10                   # this variable exists in the environment of fun_8
  
  b <<- 20                   # this variable will exist in both global 
                             # and functional environments
  
  return(x + a + b)
  
}

```

&nbsp;

### Nested Function {-}

Like `for` loops, we can produce **nested functions** by putting one function into another. Here is an example of a nested function:



``` r

fun_10 <- function(x, y){
  
  func_11 <- function(){
    
    return(x + y)
    
  }
  
  return(2*func_11())
  
}

```



``` r

fun_10(2, 3)
#> [1] 10
```
