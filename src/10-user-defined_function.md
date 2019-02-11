Goals
=====

-   Learn basic control flow: if else, for
-   Learn how to create user-defined functions

Some of the following materials are adapted from
[swcarpentry](https://github.com/swcarpentry/r-novice-gapminder) and
[hbctraining](https://github.com/hbctraining/Intro-to-R)

Basic Control-flow
==================

We will briefly discuss conditional statements and loops. They are not
only useful for writing functions, but also commonly used outside of
functions.

conditional statements: if else
-------------------------------

For conditional statements, the most commonly used approaches are the
constructs:

    # if
    if (condition is true) {
      perform action
    }

    # if ... else
    if (condition is true) {
      perform action
    } else {  # that is, if the condition is false,
      perform alternative action
    }

Say, for example, that we want R to print a message if a variable x has
a particular value:

    x <- 8

    if (x >= 10) {
      print("x is greater than or equal to 10")
    }

The print statement does not appear in the console because x is not
greater than 10. To print a different message for numbers less than 10,
we can add an else statement.

    x <- 8

    if (x >= 10) {
      print("x is greater than or equal to 10")
    } else {
      print("x is less than 10")
    }

    ## [1] "x is less than 10"

You can also test multiple conditions by using else if.

    x <- 8

    if (x >= 10) {
      print("x is greater than or equal to 10")
    } else if (x > 5) {
      print("x is greater than 5, but less than 10")
    } else {
      print("x is less than 5")
    }

    ## [1] "x is greater than 5, but less than 10"

looping: for
------------

If you want to iterate over a set of values, and perform the same
operation on each, a for() loop will do the job.

The basic structure of a for() loop is:

    for(iterator in set of values){
      do a thing
    }

For example:

    for(i in 1:10){
      print(i)
    }

    ## [1] 1
    ## [1] 2
    ## [1] 3
    ## [1] 4
    ## [1] 5
    ## [1] 6
    ## [1] 7
    ## [1] 8
    ## [1] 9
    ## [1] 10

The 1:10 bit creates a vector on the fly; you can iterate over any other
vector as well.

We will show you two ways to iterate through a vector: using indices and
using elements.

    x <- c("Boston", "Hartford", "New York")
    # iterate using indices
    for (i in 1:length(x)) {
      # using i in some fashion
      print(x[i])
    }

    ## [1] "Boston"
    ## [1] "Hartford"
    ## [1] "New York"

    # iterate using elements
    for (i in x) {
      print(i)
    }

    ## [1] "Boston"
    ## [1] "Hartford"
    ## [1] "New York"

We can use a for() loop nested within another for() loop to iterate over
two things at once.

    for(i in 1:3){
      for(j in c('a', 'b', 'c', 'd')){
        print(paste(i,j))
      }
    }

    ## [1] "1 a"
    ## [1] "1 b"
    ## [1] "1 c"
    ## [1] "1 d"
    ## [1] "2 a"
    ## [1] "2 b"
    ## [1] "2 c"
    ## [1] "2 d"
    ## [1] "3 a"
    ## [1] "3 b"
    ## [1] "3 c"
    ## [1] "3 d"

We combine the conditional statement and looping. In the following
example, we loop through a vector x, check whether each element is a
even number. Place the result ("even", "odd") in a new vector as the
output.

    x <- c(7, 2, 8, 5, 1, 3)
    output <- vector(mode = "character", length = length(x))
    for (i in 1:length(x)) {
      if (x[i] %% 2 == 0) {
        output[i] = "even"
      } else {
        output[i] = "odd"
      }
    }
    output

    ## [1] "odd"  "even" "even" "odd"  "odd"  "odd"

### break and continue

**break** allows you to stop an iteration when a condition is met;
**continue** allow you to skip an element when a condition is met.

Example: iterate through a vector of numbers and print the numbers
before the first 5.

    # break
    x <- c(7, 2, 8, 5, 1, 3)
    for (i in x) {
      if (i == 5) {
        break
      }
      print(i)
    }

    ## [1] 7
    ## [1] 2
    ## [1] 8

    # continue
    x <- c(7, 2, 8, 5, 1, 3)
    for (i in x) {
      if (i == 5) {
        next # skip 5
      } else {
        print(i)
      }
    }

    ## [1] 7
    ## [1] 2
    ## [1] 8
    ## [1] 1
    ## [1] 3

### while

**while** allows one to keep doing an operation until the condition is
broken. Refer to
[swcarpentry](https://github.com/swcarpentry/r-novice-gapminder/blob/gh-pages/_episodes/07-control-flow.md)
for details.

Write functions
===============

Predifined functions in base R or external packages can take you pretty
far but eventually you will to write your own functions to: 1) meet a
special need; 2) reduce repetation of codes.

**Whenever you need to repeat a section of code more than twice.**

How to define a function
------------------------

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

Write functions with vector input (not covered in class)
--------------------------------------------------------

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

Summary
-------

-   Use **if else** for conditional statements and **for** for looping.
-   The syntax for defining functions, default argument, free variables
    and vectorization.

Exercise
--------

1.  Define a vector x = runif(20), use a for loop to iterate through the
    vector and a) print out the indices of elements that are larger than
    0.5; b) print out elements that are larger than 0.5; *Challenge* c)
    instead of print out the elements in b), put them in a new vector

2.  Write a function that takes two vectors of the same length and
    concatenate the elements from the two vectors. For example, (1,
    2, 3) and ("a", "b", "c") will result in ("1 a", "2 b", "3 c").
    Hint: use *paste()* to concatenate two strings.

*Challenge* It is quite possible that a user of your function defined
for the last exercise accidentally used it on two vectors of different
sizes. Think about the consequences and try it out. Then search with
?stopifnot and use it to make sure your function is called correctly.

Solutions

Exercise 1

    # a)
    x = runif(20)
    for (i in 1:length(x)) {
      if (x[i] > 0.5) {
        print(i)
      }
    }

    ## [1] 1
    ## [1] 2
    ## [1] 4
    ## [1] 10
    ## [1] 11
    ## [1] 12
    ## [1] 15
    ## [1] 16
    ## [1] 17
    ## [1] 19
    ## [1] 20

    # b)
    for (i in x) {
      if (i > 0.5) {
        print(i)
      }
    }

    ## [1] 0.6211975
    ## [1] 0.6884438
    ## [1] 0.6508979
    ## [1] 0.9417318
    ## [1] 0.6021556
    ## [1] 0.584737
    ## [1] 0.9446166
    ## [1] 0.9381222
    ## [1] 0.9682017
    ## [1] 0.8856864
    ## [1] 0.8783248

    # c)
    out <- vector(mode = "numeric")
    n = 1 # we use n to track the position of newly added elements in out
    for (i in x) {
      if (i > 0.5) {
        out[n] = i
        n = n + 1 # update n
      }
    }

    out

    ##  [1] 0.6211975 0.6884438 0.6508979 0.9417318 0.6021556 0.5847370 0.9446166
    ##  [8] 0.9381222 0.9682017 0.8856864 0.8783248

Exercise 2

    f <- function(x, y) {
      size = length(x)
      output = vector(mode = "character", length = size)
      for (i in 1:length(x)) {
        output[i] = paste(x[i], y[i])
      }
      output
    }

    #test
    f(c(1,2,3), c("a", "b", "c"))

    ## [1] "1 a" "2 b" "3 c"

    #what do the following calls return and why?
    #f(c(1,2,3), c("a", "b"))
    #f(c(1,2,3), c("a", "b", "c", "d"))

Challenge

    f <- function(x, y) {
      stopifnot(length(x) == length(y))
      size = length(x)
      output = vector(mode = "character", length = size)
      for (i in 1:length(x)) {
        output[i] = paste(x[i], y[i])
      }
      output
    }

    #test
    f(c(1,2,3), c("a", "b", "c"))

    ## [1] "1 a" "2 b" "3 c"

    # The following call will result in an error
    #f(c(1,2,3), c("a", "b", "c", "d"))
