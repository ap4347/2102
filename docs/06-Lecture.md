# Lecture 06 {-}

&nbsp;

## Functions {-}


Now, it’s time to discuss one of the most essential concepts of R programming, that is, `R functions`. Writing functions is a core activity of an R programmer. Functions are often used to encapsulate a sequence of expressions that need to be executed numerous times, perhaps under slightly different conditions. In other words, functions allow us to automate common tasks in a more powerful and general way than simply copy-and-pasting. They are also often written when code must be shared with others or the public.


You should consider writing a function whenever you’ve copied and pasted a block of code more than twice. In this class, we will learn how to create a customized functions and will discuss essential tools that `R functions` are equipped with. 

### Creating a Function {-}

A function is a set of statements organized together to perform a specific task. R has a large number of in-built functions and the user can create their own functions. In R, a function is an object so the R interpreter is able to pass control to the function, along with arguments that may be necessary for the function to accomplish the actions.


An `R function` is created by using the `function()` function (Yeah, too many functions). The basic syntax of an R function is as follows:



```r

function_name <- function(arg_1, arg_2, ...) {
  
   Function body 
}

```

The main components of an R function are:

* `Function Name` - this is the name of your function. It is stored in R environment as an object under this name.
* `Arguments` - an argument is a placeholder. When a function is invoked, you pass a value to the argument. Arguments are optional; that is, a function may contain no arguments. Also arguments can have default values. In this example, arguments are `arg_1`, 'arg_2` etc.
* `Function Body` − the function body contains a collection of statements that defines what the function does.
* `Return Value` − the return value of a function is the last expression in the function body to be evaluated.
