7-datastructures-lists
================
Sandeep Namburi
February 04, 2019

### Lists

Lists
-----

In R lists act as containers. Unlike atomic vectors, the contents of a list are not restricted to a single mode and can encompass any mixture of data types. Lists are sometimes called generic vectors, because the elements of a list can by of any type of R object, even lists containing further lists. This property makes them fundamentally different from atomic vectors.

A list is a special type of vector. Each element can be a different type.

Create lists using `list()` or coerce other objects using `as.list()`. An empty list of the required length can be created using `vector()`

``` r
x <- list(1, "a", TRUE, 1+4i)
x
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] "a"
    ## 
    ## [[3]]
    ## [1] TRUE
    ## 
    ## [[4]]
    ## [1] 1+4i

``` r
x <- vector("list", length = 5) # empty list
length(x)
```

    ## [1] 5

The content of elements of a list can be retrieved by using double square brackets.

``` r
x[[1]]
```

    ## NULL

Vectors can be coerced to lists as follows:

``` r
x <- 1:10
x <- as.list(x)
length(x)
```

    ## [1] 10

1.  What is the class of `x[1]`?
2.  What about `x[[1]]`?

Elements of a list can be named (i.e. lists can have the `names` attribute)

``` r
xlist <- list(a = "Karthik Ram", b = 1:10, data = head(iris))
xlist
```

    ## $a
    ## [1] "Karthik Ram"
    ## 
    ## $b
    ##  [1]  1  2  3  4  5  6  7  8  9 10
    ## 
    ## $data
    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

``` r
names(xlist)
```

    ## [1] "a"    "b"    "data"

1.  What is the length of this object? What about its structure?

Lists can be extremely useful inside functions. Because the functions in R are able to return only a single object, you can "staple" together lots of different kinds of results into a single object that a function can return.

A list does not print to the console like a vector. Instead, each element of the list starts on a new line.

Elements are indexed by double brackets. Single brackets will still return a(nother) list. If the elements of a list are named, they can be referenced by the `$` notation (i.e. `xlist$data`).

------------------------------------------------------------------------

### Writing to file

Everything we have done so far has only modified the data in R; the files have remained unchanged. Whenever we want to save our datasets to file, we need to use a `write` function in R.

To write our matrix to file in comma separated format (.csv), we can use the `write.csv` function. There are two required arguments: the variable name of the data structure you are exporting, and the path and filename that you are exporting to. By default the delimiter is set, and columns will be separated by a comma:

``` r
write.csv(data, file="subset_meta.csv")
```

Similar to reading in data, there are a wide variety of functions available allowing you to export data in specific formats. Another commonly used function is `write.table`, which allows you to specify the delimiter you wish to use. This function is commonly used to create tab-delimited files.

> **NOTE:** Sometimes when writing a dataframe with row names to file, the column names will align starting with the row names column. To avoid this, you can include the argument `col.names = NA` when writing to file to ensure all of the column names line up with the correct column values.

Writing a vector of values to file requires a different function than the functions available for writing dataframes. You can use `write()` to save a vector of values to file. For example:

------------------------------------------------------------------------

> ### An R package for data wrangling
>
> The methods presented above are using base R functions for data wrangling. Later we will explore the **Tidyverse suite of packages**, specifically designed to make data wrangling easier.

------------------------------------------------------------------------

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

-   *The materials used in this lesson are adapted from work that is Copyright © Data Carpentry (<http://datacarpentry.org/>). All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
