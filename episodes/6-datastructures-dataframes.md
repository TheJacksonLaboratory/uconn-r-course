6-datastructures-dataframes
================
Sandeep Namburi
2/2/2019

------------------------------------------------------------------------

The materials used in this lesson are adapted from work from both members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/) and Data Carpentry (<http://datacarpentry.org/>).

-   HBC Intro to R Github: <https://github.com/hbctraining/Intro-to-R>
-   Data Carpentry Intro to R: <https://github.com/swcarpentry/r-novice-gapminder>

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

-   *The materials used in this lesson are adapted from work that is Copyright © Data Carpentry (<http://datacarpentry.org/>). All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*

------------------------------------------------------------------------

Data frame
----------

A data frame is a very important data type in R. It's pretty much the *de facto* data structure for most tabular data and what we use for statistics.

A data frame is a special type of list where every element of the list has same length.

Data frames can have additional attributes such as `rownames()`, which can be useful for annotating data, like `subject_id` or `sample_id`. But most of the time they are not used.

Some additional information on data frames:

-   Usually created by `read.csv()` and `read.table()`.
-   Can convert to matrix with `data.matrix()` (preferred) or `as.matrix()`
-   Coercion will be forced and not always what you expect.
-   Can also create with `data.frame()` function.
-   Find the number of rows and columns with `nrow(dat)` and `ncol(dat)`, respectively.
-   Rownames are usually 1, 2, ..., n.

#### Creating data frames by hand

``` r
dat <- data.frame(id = letters[1:10], x = 1:10, y = 11:20)
dat
```

    ##    id  x  y
    ## 1   a  1 11
    ## 2   b  2 12
    ## 3   c  3 13
    ## 4   d  4 14
    ## 5   e  5 15
    ## 6   f  6 16
    ## 7   g  7 17
    ## 8   h  8 18
    ## 9   i  9 19
    ## 10  j 10 20

#### Useful data frame functions

-   `head()` - shown first 6 rows
-   `tail()` - show last 6 rows
-   `dim()` - returns the dimensions
-   `nrow()` - number of rows
-   `ncol()` - number of columns
-   `str()` - structure of each column
-   `names()` - shows the `names` attribute for a data frame, which gives the column names.

See that it is actually a special list:

``` r
is.list(iris)
```

    ## [1] TRUE

``` r
class(iris)
```

    ## [1] "data.frame"

| Dimensions | Homogenous    | Heterogeneous |
|------------|---------------|---------------|
| 1-D        | atomic vector | list          |
| 2-D        | matrix        | data frame    |

Learning Objectives
-------------------

-   Demonstrate how to subset, merge, and create new datasets from existing data structures in R.
-   Export data tables and plots for use outside of the R environment.

### Dataframes

Dataframes (and matrices) have 2 dimensions (rows and columns), so if we want to select some specific data from it we need to specify the "coordinates" we want from it. We use the same square bracket notation but rather than providing a single index, there are *two indices required*. Within the square bracket, **row numbers come first followed by column numbers (and the two are separated by a comma)**. Let's explore the `data` dataframe:

For example:

``` r
data <- read.csv(file="surveys.csv")

data[1, 1]   # element from the first row in the first column of the data frame
```

    ## [1] 1

``` r
data[1, 3]   # element from the first row in the 3rd column
```

    ## [1] 16

Now if you only wanted to select based on rows, you would provide the index for the rows and leave the columns index blank. The key here is to include the comma, to let R know that you are accessing a 2-dimensional data structure:

``` r
data[3, ]    # vector containing all elements in the 3rd row
```

If you were selecting specific columns from the data frame - the rows are left blank:

``` r
data[ , 3]    # vector containing all elements in the 3rd column
```

Just like with vectors, you can select multiple rows and columns at a time. Within the square brackets, you need to provide a vector of the desired values:

``` r
data[ , 1:2] # dataframe containing first two columns
data[c(1,3,6), ] # dataframe containing first, third and sixth rows
```

For larger datasets, it can be tricky to remember the column number that corresponds to a particular variable.

``` r
data[1:3 , "month"] # elements of the celltype column corresponding to the first three samples
```

You can do operations on a particular column, by selecting it using the `$` sign. In this case, the entire column is a vector. For instance, to extract all the genotypes from our dataset, we can use:

``` r
data$species_id 
```

You can use `colnames(data)` or `names(data)` to remind yourself of the column names. We can then supply index values to select specific values from that vector. For example, if we wanted the genotype information for the first five samples in `data`:

``` r
colnames(data)

data$hindfoot_length[1:5]
```

The `$` allows you to select a single column by name. To select multiple columns by name, you need to concatenate a vector of strings that correspond to column names:

``` r
head(data[, c("year", "plot_id")])
```

``` r
year plot_id
1977    2           
1977    3           
1977    2           
1977    7           
1977    3           
1977    1           
1977    2           
1977    1           
1977    1           
1977    6   
```

While there is no equivalent `$` syntax to select a row by name, you can select specific rows using the row names. To remember the names of the rows, you can use the `rownames()` function:

``` r
rownames(data)

data[c("month", "day"),]
```

#### Selecting using indices with logical operators

With dataframes, similar to vectors, we can use logical vectors for specific columns in the dataframe to select only the rows in a dataframe with TRUE values at the same position or index as in the logical vector. We can then use the logical vector to return all of the rows in a dataframe where those values are TRUE.

``` r
idx <- data$sex == "F"
    
data[idx, ]
```

##### Selecting indices with logical operators using the `which()` function

As you might have guessed, we can also use the `which()` function to return the indices for which the logical expression is TRUE. For example, we can find the indices where the `celltype` is `typeA` within the `data` dataframe:

``` r
idx <- which(data$month == "7")
    
data[idx, ]
```

Or we could find the indices for the data replicates 2 and 3:

``` r
idx <- which(data$weight > 10)
    
data[idx, ]
```

------------------------------------------------------------------------

**Exercise**

Subset the `data` dataframe to return only the rows of data with a genotype of `KO`.

------------------------------------------------------------------------

> **NOTE:** There are easier methods for subsetting **dataframes** using logical expressions, including the `filter()` and the `subset()` functions. These functions will return the rows of the dataframe for which the logical expression is TRUE, allowing us to subset the data in a single step. We will explore the `filter()` function in more detail in a later lesson.
