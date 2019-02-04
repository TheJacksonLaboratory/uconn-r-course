5-datastructures-matrix
================
Sandeep Namburi
2/3/2019

------------------------------------------------------------------------

The materials used in this lesson are adapted from work from both members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/) and Data Carpentry (<http://datacarpentry.org/>).

-   HBC Intro to R Github: <https://github.com/hbctraining/Intro-to-R>
-   Data Carpentry Intro to R: <https://github.com/swcarpentry/r-novice-gapminder>

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

-   *The materials used in this lesson are adapted from work that is Copyright © Data Carpentry (<http://datacarpentry.org/>). All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*

------------------------------------------------------------------------

Matrices
--------

A matrix is a bi-dimensional collection of data:

``` r
a <- matrix(1:12, nrow=3, ncol=4) # define a matrix with 3 rows and 4 columns
a
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    4    7   10
    ## [2,]    2    5    8   11
    ## [3,]    3    6    9   12

We can declare a matrix full of zeros:

``` r
matrix_example <- matrix(0, ncol=6, nrow=3)
matrix_example
```

    ##      [,1] [,2] [,3] [,4] [,5] [,6]
    ## [1,]    0    0    0    0    0    0
    ## [2,]    0    0    0    0    0    0
    ## [3,]    0    0    0    0    0    0

And similar to other data structures, we can ask things about our matrix:

``` r
class(matrix_example)
```

    ## [1] "matrix"

``` r
typeof(matrix_example)
```

    ## [1] "double"

``` r
str(matrix_example)
```

    ##  num [1:3, 1:6] 0 0 0 0 0 0 0 0 0 0 ...

``` r
dim(matrix_example)
```

    ## [1] 3 6

``` r
nrow(matrix_example)
```

    ## [1] 3

``` r
ncol(matrix_example)
```

    ## [1] 6

> Challenge 4
> -----------
>
> What do you think will be the result of `length(matrix_example)`? Try it. Were you right? Why / why not?
>
> > Solution to Challenge 4
> > -----------------------
> >
> > What do you think will be the result of `length(matrix_example)`?
> >
> > ``` r
> > matrix_example <- matrix(0, ncol=6, nrow=3)
> > length(matrix_example)
> > ```
> >
> >     ## [1] 18
> >
> > Because a matrix is a vector with added dimension attributes, `length` gives you the total number of elements in the matrix. {: .solution} {: .challenge}

> Challenge 5
> -----------
>
> Make another matrix, this time containing the numbers 1:50, with 5 columns and 10 rows. Did the `matrix` function fill your matrix by column, or by row, as its default behaviour? See if you can figure out how to change this. (hint: read the documentation for `matrix`!)
>
> > Solution to Challenge 5
> > -----------------------
> >
> > Make another matrix, this time containing the numbers 1:50, with 5 columns and 10 rows. Did the `matrix` function fill your matrix by column, or by row, as its default behaviour? See if you can figure out how to change this. (hint: read the documentation for `matrix`!)
> >
> > ``` r
> > x <- matrix(1:50, ncol=5, nrow=10)
> > x <- matrix(1:50, ncol=5, nrow=10, byrow = TRUE) # to fill by row
> > ```
> >
> > {: .solution} {: .challenge}

> Challenge 6
> -----------
>
> Create a list of length two containing a character vector for each of the sections in this part of the workshop:
>
> -   Data types
> -   Data structures
>
> Populate each character vector with the names of the data types and data structures we've seen so far.
>
> > Solution to Challenge 6
> > -----------------------
> >
> > ``` r
> > dataTypes <- c('double', 'complex', 'integer', 'character', 'logical')
> > dataStructures <- c('data.frame', 'vector', 'factor', 'list', 'matrix')
> > answer <- list(dataTypes, dataStructures)
> > ```
> >
> > Note: it's nice to make a list in big writing on the board or taped to the wall listing all of these types and structures - leave it up for the rest of the workshop to remind people of the importance of these basics.
> >
> > {: .solution} {: .challenge}

> Challenge 7
> -----------
>
> Consider the R output of the matrix below:
>
>     ##      [,1] [,2]
>     ## [1,]    4    1
>     ## [2,]    9    5
>     ## [3,]   10    7
>
> What was the correct command used to write this matrix? Examine each command and try to figure out the correct one before typing them. Think about what matrices the other commands will produce.
>
> 1.  `matrix(c(4, 1, 9, 5, 10, 7), nrow = 3)`
> 2.  `matrix(c(4, 9, 10, 1, 5, 7), ncol = 2, byrow = TRUE)`
> 3.  `matrix(c(4, 9, 10, 1, 5, 7), nrow = 2)`
> 4.  `matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)`
>
> > Solution to Challenge 7
> > -----------------------
> >
> > Consider the R output of the matrix below:
> >
> >     ##      [,1] [,2]
> >     ## [1,]    4    1
> >     ## [2,]    9    5
> >     ## [3,]   10    7
> >
> > What was the correct command used to write this matrix? Examine each command and try to figure out the correct one before typing them. Think about what matrices the other commands will produce.
> >
> > ``` r
> > matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)
> > ```
> >
> > {: .solution} {: .challenge}
