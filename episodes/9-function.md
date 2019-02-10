Goal
====

-   Describe and utilize functions in R.
-   Modify default behavior of functions using arguments in R.

Functions
=========

A key feature of R is functions. Functions are **"self contained"
modules of code that accomplish a specific task**. Functions usually
take in some sort of data structure (value, vector, dataframe etc.),
process it, and return a result.

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
the square root of (a).

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
about the round function.We see that if we want a different number of
digits, we can type digits=2 or however many we want.

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

    surveys <- read.csv("surveys.csv")
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
overwriting the default option na.rm.

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

    #output <- c(sum_lengths, mean_lengths, sd_lengths, median_lengths, max_lengths, min_lengths)
    #names(output) <- c("sum", "mean", "sd", "median", "max", "min")
    #output

Note: R functions work on vectors. For example, if you pass a vector of
numbers to sqrt(), it will take square root of each element and return
them as a vector.

    random_numbers <- runif(5, min = 0, max = 100)
    sqrt(random_numbers)

    ## [1] 8.928925 8.816465 9.696636 8.602128 7.832343

    round(random_numbers, 1)

    ## [1] 79.7 77.7 94.0 74.0 61.3
