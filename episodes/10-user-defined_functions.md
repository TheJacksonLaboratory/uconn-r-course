Goal
====

-   Learn how to create user-defined functions in R

Predifined functions in base R or external packages can take you pretty
far but eventually you will to write your own functions to: 1) meet a
special need; 2) reduce repetation of codes.

**Whenever you need to repeat a section of code more than twice.**

How to define a function
========================

    function_name <- function(arg1, arg2, arg3, ...) {
        # do something interesting
        # return the value
    }

The return value is the last expression in the function body.

example 1
---------

Write a function to convert fahrenheit to celcius. Hint: c = (f - 32) \*
5/9

    fahr_to_cels <- function(x) {
        (x - 32) * 5 / 9
    }

    fahr_to_cels(32)

    ## [1] 0

    fahr_to_cels(68)

    ## [1] 20

    fahr_to_cels(100)

    ## [1] 37.77778

Challenge:

Write a function to convert celcius to fahrenheit.

Chaining functions
------------------

Functions allow us to divide a large task into smaller modules that are
easier to create.

Write a function to convert celsius to kalvin.

    cels_to_kalv <- function(x) {
      x + 273.15
    }

How to convert falrenheit to kalvin? We already know how to convert
fahrenheit to celcius and celcius to kavelin.

    fahr_to_kalv <- function(x) {
      cels <- fahr_to_cels(x)
      kelv <- cels_to_kalv(cels)
      kelv
    }

Default values
--------------

Named arguments are useful when you have a long list of arguments and
you want to use the defaults for everything except for a few

    fruit_expense <- function(apple, orange, price_apple = 0.75, price_orange = 0.99) {
        apple_expense <- apple * price_apple
        orange_expense <- orange * price_orange
        apple_expense + orange_expense
    }

    fruit_expense(3, 4)

    ## [1] 6.21

scoping
-------

Consider the following function

    foo <- function(x, y) {
        x ^ 2 + y / z
    }

This function has 2 formal arguments, x and y. In the body of the
function, there is another symbol z. In this case, z is called a *free
viariable*.

How does R find the value of z? R first searches the body of the
function. If R cannot find the binding, it searches the global
environment (\* search for environment for yourself.

Do not abuse free variables as it can be difficult to debug!

Write functions with vector input
---------------------------------

Write a function that convert positive numbers and 0 to their square
root, and do nothing to negative numbers.

    my_sqrt <- function(x) {
      if (x >= 0) {
        result <- sqrt(x)
      } else {
        result <- x
      }
      result
    }

The above definition works, but only when the input is a single numeric
value. We'd like the function to take vectors as that is often desired
for working with dataframes.

    my_sqrt <- function(x) {
      result <- vector(mode = "numeric", length = length(x))
      for (i in 1:length(x)) {
        if (x[i] >= 0) {
          result[i] <- sqrt(x[i])
        } else {
          result[i] <- x[i]
        }
      }
      result
    }
