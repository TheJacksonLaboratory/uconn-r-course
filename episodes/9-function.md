Goals
=====

-   Describe and utilize functions in R.
-   Modify default behavior of functions using arguments in R.
-   Describe how R matches arguments
-   Practice commonly used functions

Some of the following materials are adapted from
[swcarpentry](https://github.com/swcarpentry/r-novice-gapminder) and
[hbctraining](https://github.com/hbctraining/Intro-to-R)

Functions
=========

A key feature of R is functions. Functions are self contained modules of
code that accomplish a specific task. Functions usually take in some
sort of data structure (value, vector, dataframe etc.), process it, and
return a result.

The general usage for a function is the name of the function followed by
parentheses:

    function_name(input)

Arguments
---------

The input(s) are called arguments, which can include:

-   the physical object (any data structure) on which the function
    carries out a task
-   specifications that alter the way the function operates (e.g.
    options)

In the following example, *sqrt* requires one argument (a) and return
the square root.

    a <- 16
    b <- sqrt(a)
    b

    ## [1] 4

Not all functions take arguments, for example:

    getwd()

However, most functions can take several arguments. If you don't specify
all arguments when calling the function, you will either receive an
error or the function will fall back on using a default.

The defaults represent standard values that the author of the function
specified as being "good enough in standard cases". However, if you want
something specific, simply change the argument yourself with a value of
your choice.

An example is the *round()* function: the default value for *digits* is
0.

    round(x, digits = 0)

Here, we’ve called round() with just one argument, 3.14159, and it has
returned the value 3. That’s because the default is to round to the
nearest whole number.

    round(3.1415926)

    ## [1] 3

If we want more digits we can see how to do that by getting information
about the round function. We see that if we want a different number of
digits, we can type *digits=2* or other decimal places.

    ?round
    round(3.1415926, digits = 2)

    ## [1] 3.14

Argument matching
-----------------

When a function takes in multiple arguments, R matches the values to the
arguments by their names, or by their position. The following calls are
identical:

    # match by name
    round(x = 3.1415926, digits = 2)

    ## [1] 3.14

    round(digits = 2, x = 3.1415926)

    ## [1] 3.14

    # match by position
    round(3.1415926, 2)

    ## [1] 3.14

    # mix name and position
    round(3.1415926, digits = 2)

    ## [1] 3.14

    round(digits = 2, 3.1415926)

    ## [1] 3.14

    # okay but avoid partial name matching
    round(3.1415926, d = 2)

    ## [1] 3.14

    round(3.1415926, digi = 2)

    ## [1] 3.14

Being explicit can help improve the readability of your code.

Basic functions
---------------

We have already used a few examples of basic functions in the previous
lessons i.e getwd(), sqrt(), and factor(). These functions are available
as part of R's built in capabilities, and we will explore a few more of
these base functions below.

    surveys <- read.csv("../surveys.csv")
    lengths <- surveys$hindfoot_length
    head(lengths)

    ## [1] 32 33 37 36 35 14

    tail(lengths)

    ## [1] NA NA NA 15 36 NA

    summary(lengths)

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    2.00   21.00   32.00   29.29   36.00   70.00    4111

We will illustrate some commonly used statistics functions to *lengths*.

    sum_lengths <- sum(lengths)
    sum_lengths

    ## [1] NA

*Alert: Missing values!* If you have missing values (**NA**), the
functions will always return **NA**. The solution is to remove NA by
overwriting the default option na.rm from **FALSE** to **TRUE**.

    sum_lengths <- sum(lengths, na.rm = TRUE)
    sum_lengths

    ## [1] 920754

    mean_lengths <- mean(lengths, na.rm = TRUE)
    mean_lengths

    ## [1] 29.28793

    sd_lengths <- sd(lengths, na.rm = TRUE)
    sd_lengths

    ## [1] 9.564759

    median_lengths <- median(lengths, na.rm = TRUE)
    median_lengths

    ## [1] 32

    max_lengths <- max(lengths, na.rm = TRUE)
    max_lengths

    ## [1] 70

    min_lengths <- min(lengths, na.rm = TRUE)
    min_lengths

    ## [1] 2

Nested function call
--------------------

Thus far, to perform any specific task, we have executed every function
separately; if we wanted to use the results of a function for downstream
purposes, we saved the results to a variable. As you become more
comfortable with R, you will find that it is more efficient to code
using nested functions, or functions within other functions, which will
allow you to execute multiple commands at the same time.

Here is an example. We want to calculate the mean of length, and then
round the number to 2 decimal places. The following solutions are
equivalent.

    # solution 1
    mean_lengths <- mean(lengths, na.rm = TRUE)
    round(mean_lengths, 2)

    ## [1] 29.29

    # solution 2
    round(mean(lengths, na.rm = TRUE), 2)

    ## [1] 29.29

Here is another example. Count the number of NA in lengths. *is.na()*
will test each element of a vector and return a logical vector of the
same length, in which TRUE means the value at that position is NA and
FALSE otherwise. We then call *sum()* on the vector to get the count of
TRUEs (remember TRUE is 1 and FALSE is 0 in calculation).

    sum(is.na(lengths))

    ## [1] 4111

To improve code readibility, do not chain TOO many function calls!

Vectorization
-------------

Most of R's functions are vectorized, meaning that the function will
operate on all elements of a vector without needing to loop through and
act on each element one at a time. This makes writing code more concise,
easy to read, and less error prone.

For example, if you pass a vector of numbers to sqrt(), it will take
square root of each element and return them as a vector.

    random_numbers <- c(23.7, 16, 5.6, 90, 2)
    sqrt(random_numbers)

    ## [1] 4.868265 4.000000 2.366432 9.486833 1.414214

R operators are also functions, and they work on vectors too. When you
add a number to a vector, R adds the number to each element of the
vector.

    x <- 1:4
    x * 10

    ## [1] 10 20 30 40

The multiplication happened to each element of the vector.

We can also add two vectors together:

    y <- 5:8
    x + y

    ## [1]  6  8 10 12

Each element of x was added to its corresponding element of y:

    x:   1  2  3  4
         +  +  +  +
    y:   5  6  7  8
    ----------------
         6  8  10 12

Comparison operators and logical operators are also vectorized:

    x > 2

    ## [1] FALSE FALSE  TRUE  TRUE

    (x > 2) & (x < 4)

    ## [1] FALSE FALSE  TRUE FALSE

Summary
-------

-   Functions are modules of codes for certain task.
-   Functions can have 0, 1 or multiple arguments. Some arguments are
    required, and others are optional with a default value.
-   R matches function arguments by name or position.
-   Pay attention to NAs when calling functions.
-   Many functions work on vectors.

Exercise
--------

1.  The runif() function generates random number within a certain range.
    Use ?runif to lookup the function, and a) return 10 random numbers
    with the default options; b) return 9 random numbers between 0.5
    and 0.6.

2.  Read in surveys data, extract the weight column and assign it to a
    variable, then find the sum, mean, sd, min, max of it.

3.  We want to know the value t = 1 / (2 \* 3) + 1 / (3 \* 3) + 1 /
    (4 \* 3) + ... + 1 / (50 \* 3). Use vectorization to calculate it
    (round to the fourth digit)

Solutions

Exercise 1

    # a)
    runif(10)

    ##  [1] 0.7942200 0.8665162 0.8256717 0.6199974 0.8969318 0.3386191 0.1312040
    ##  [8] 0.5679401 0.9667932 0.1868094

    # b)
    runif(10, min = 0.5, max = 0.6)

    ##  [1] 0.5511370 0.5670234 0.5950174 0.5239278 0.5599547 0.5911921 0.5662101
    ##  [8] 0.5972820 0.5489147 0.5062840

Exercise 2

    surveys <- read.csv("../surveys.csv")
    weights <- surveys$weight
    sum(weights, na.rm = TRUE)

    ## [1] 1377594

    mean(weights, na.rm = TRUE)

    ## [1] 42.67243

    sd(weights, na.rm = TRUE)

    ## [1] 36.63126

    median(weights, na.rm = TRUE)

    ## [1] 37

    max(weights, na.rm = TRUE)

    ## [1] 280

    min(weights, na.rm = TRUE)

    ## [1] 4

Exercise 3

    n <- 2:50
    t <- sum(1 / (n * 3))
    round(t, 4)

    ## [1] 1.1664

    # or
    round(sum(1 / ((2:50) * 3)), 4)

    ## [1] 1.1664
