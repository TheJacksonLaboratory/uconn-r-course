2-datatypes
================
Sandeep Namburi
February 04, 2019

------------------------------------------------------------------------

The materials used in this lesson are adapted from work from both members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/) and Data Carpentry (<http://datacarpentry.org/>).

-   HBC Intro to R Github: <https://github.com/hbctraining/Intro-to-R>
-   Data Carpentry Intro to R: <https://github.com/swcarpentry/r-novice-gapminder>

*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

-   *The materials used in this lesson are adapted from work that is Copyright © Data Carpentry (<http://datacarpentry.org/>). All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*

------------------------------------------------------------------------

Learning Objectives
-------------------

-   Construct data structures to store external data in R.
-   Inspect data structures in R.
-   Demonstrate how to subset data from data structures.

Reading data into R
-------------------

Regardless of the specific analysis in R we are performing, we usually need to bring data in for the analysis. The function in R we use will depend on the type of data file we are bringing in (e.g. text, Stata, SPSS, SAS, Excel, etc.) and how the data in that file are separated, or delimited. The table below lists functions that can be used to import data from common file formats.

| Data type               | Extension | Function          | Package            |
|:------------------------|:----------|:------------------|:-------------------|
| Comma separated values  | csv       | `read.csv()`      | utils (default)    |
|                         |           | `read_csv()`      | readr (tidyverse)  |
| Tab separated values    | tsv       | `read_tsv()`      | readr              |
| Other delimited formats | txt       | `read.table()`    | utils              |
|                         |           | `read_table()`    | readr              |
|                         |           | `read_delim()`    | readr              |
| Stata version 13-14     | dta       | `readdta()`       | haven              |
| Stata version 7-12      | dta       | `read.dta()`      | foreign            |
| SPSS                    | sav       | `read.spss()`     | foreign            |
| SAS                     | sas7bdat  | `read.sas7bdat()` | sas7bdat           |
| Excel                   | xlsx, xls | `read_excel()`    | readxl (tidyverse) |

For example, if we have text file separated by commas (comma-separated values), we could use the function `read.csv`. However, if the data are separated by a different delimiter in a text file, we could use the generic `read.table` function and specify the delimiter as an argument in the function.

When working with genomic data, we often have a metadata file containing information on each sample in our dataset. Let's bring in the metadata file using the `read.csv` function. Check the arguments for the function to get an idea of the function options:

``` r
?read.csv
```

The `read.csv` function has *one required argument* and several *options* that can be specified. The mandatory argument is a path to the file and filename, which in our case is `data/mouse_exp_design.csv`. We will put the function to the right of the assignment operator, meaning that **any output will be saved as the variable name provided on the left**.

``` r
data <- read.csv(file="surveys.csv")
```

> *Note: By default, `read.csv` converts (= coerces) columns that contain characters (i.e., text) into the `factor` data type. Depending on what you want to do with the data, you may want to keep these columns as `character`. To do so, `read.csv()` and `read.table()` have an argument called `stringsAsFactors` which can be set to `FALSE`.*

Inspecting data structures
--------------------------

There are a wide selection of base functions in R that are useful for inspecting your data and summarizing it. Let's use the `data` file that we created to test out data inspection functions.

Take a look at the dataframe by typing out the variable name `data` and pressing return:

``` r
data
```

This is top few lines of the dataframe

    ##   record_id month day year plot_id species_id sex hindfoot_length weight
    ## 1         1     7  16 1977       2         NL   M              32     NA
    ## 2         2     7  16 1977       3         NL   M              33     NA
    ## 3         3     7  16 1977       2         DM   F              37     NA
    ## 4         4     7  16 1977       7         DM   M              36     NA
    ## 5         5     7  16 1977       3         DM   M              35     NA
    ## 6         6     7  16 1977       1         PF   M              14     NA

Suppose we had a larger file, we might not want to display all the contents in the console. Instead we could check the top (the first 6 lines) of this `data.frame` using the function `head()`:

``` r
head(data)
```

    ##   record_id month day year plot_id species_id sex hindfoot_length weight
    ## 1         1     7  16 1977       2         NL   M              32     NA
    ## 2         2     7  16 1977       3         NL   M              33     NA
    ## 3         3     7  16 1977       2         DM   F              37     NA
    ## 4         4     7  16 1977       7         DM   M              36     NA
    ## 5         5     7  16 1977       3         DM   M              35     NA
    ## 6         6     7  16 1977       1         PF   M              14     NA

Previously, we had mentioned that character values get converted to factors by default using `data.frame`. One way to assess this change would be to use the \_\_`str`\_\_ucture function. You will get specific details on each column:

``` r
str(data)
```

    ## 'data.frame':    35549 obs. of  9 variables:
    ##  $ record_id      : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ month          : int  7 7 7 7 7 7 7 7 7 7 ...
    ##  $ day            : int  16 16 16 16 16 16 16 16 16 16 ...
    ##  $ year           : int  1977 1977 1977 1977 1977 1977 1977 1977 1977 1977 ...
    ##  $ plot_id        : int  2 3 2 7 3 1 2 1 1 6 ...
    ##  $ species_id     : Factor w/ 49 levels "","AB","AH","AS",..: 17 17 13 13 13 24 23 13 13 24 ...
    ##  $ sex            : Factor w/ 3 levels "","F","M": 3 3 2 3 3 3 2 3 2 2 ...
    ##  $ hindfoot_length: int  32 33 37 36 35 14 NA 37 34 20 ...
    ##  $ weight         : int  NA NA NA NA NA NA NA NA NA NA ...

As you can see, the columns `species_id` and `sex` are of the `factor` class, whereas the `weight` column has been interpreted as integer data type.

**You can also get this information from the "Environment" tab in RStudio.**

You can try `summary`:

``` r
summary(data)
```

    ##    record_id         month             day             year     
    ##  Min.   :    1   Min.   : 1.000   Min.   : 1.00   Min.   :1977  
    ##  1st Qu.: 8888   1st Qu.: 4.000   1st Qu.: 9.00   1st Qu.:1984  
    ##  Median :17775   Median : 6.000   Median :16.00   Median :1990  
    ##  Mean   :17775   Mean   : 6.474   Mean   :16.11   Mean   :1990  
    ##  3rd Qu.:26662   3rd Qu.: 9.000   3rd Qu.:23.00   3rd Qu.:1997  
    ##  Max.   :35549   Max.   :12.000   Max.   :31.00   Max.   :2002  
    ##                                                                 
    ##     plot_id       species_id    sex       hindfoot_length     weight      
    ##  Min.   : 1.0   DM     :10596    : 2511   Min.   : 2.00   Min.   :  4.00  
    ##  1st Qu.: 5.0   PP     : 3123   F:15690   1st Qu.:21.00   1st Qu.: 20.00  
    ##  Median :11.0   DO     : 3027   M:17348   Median :32.00   Median : 37.00  
    ##  Mean   :11.4   PB     : 2891             Mean   :29.29   Mean   : 42.67  
    ##  3rd Qu.:17.0   RM     : 2609             3rd Qu.:36.00   3rd Qu.: 48.00  
    ##  Max.   :24.0   DS     : 2504             Max.   :70.00   Max.   :280.00  
    ##                 (Other):10799             NA's   :4111    NA's   :3266

Note the `sex` and `species_id` factor column, we will come back to it later

### List of functions for data inspection

We already saw how the functions `head()` and `str()` can be useful to check the content and the structure of a `data.frame`. Here is a non-exhaustive list of functions to get a sense of the content/structure of data.

-   All data structures - content display:
    -   **`str()`:** compact display of data contents (env.)
    -   **`class()`:** data type (e.g. character, numeric, etc.) of vectors and data structure of dataframes, matrices, and lists.
    -   **`summary()`:** detailed display, including descriptive statistics, frequencies
    -   **`head()`:** will print the beginning entries for the variable
    -   **`tail()`:** will print the end entries for the variable
-   Vector and factor variables:
    -   **`length()`:** returns the number of elements in the vector or factor
-   Dataframe and matrix variables:
    -   **`dim()`:** returns dimensions of the dataset
    -   **`nrow()`:** returns the number of rows in the dataset
    -   **`ncol()`:** returns the number of columns in the dataset
    -   **`rownames()`:** returns the row names in the dataset
    -   **`colnames()`:** returns the column names in the dataset

``` r
char_plus_flot <- 'black' + 2.1
```

    ## Error in "black" + 2.1: non-numeric argument to binary operator

Data Types
----------

If you guessed that the last command will return an error because `2.1` plus `"black"` is nonsense, you're right - and you already have some intuition for an important concept in programming called *data types*. We can ask what type of data something is:

``` r
typeof(data$weight)
```

    ## [1] "integer"

There are 5 main types: `double`, `integer`, `complex`, `logical` and `character`.

``` r
typeof(3.14)
```

    ## [1] "double"

``` r
typeof(1L) # The L suffix forces the number to be an integer, since by default R uses float numbers
```

    ## [1] "integer"

``` r
typeof(1+1i)
```

    ## [1] "complex"

``` r
typeof(TRUE)
```

    ## [1] "logical"

``` r
typeof('banana')
```

    ## [1] "character"

No matter how complicated our analyses become, all data in R is interpreted as one of these basic data types. This strictness has some really important consequences.

``` r
file.show("surveys.csv")
```

``` r
record_id,month,day,year,plot_id,species_id,sex,hindfoot_length,weight
1,7,16,1977,2,NL,M,32,
2,7,16,1977,3,NL,M,33,
3,7,16,1977,2,DM,F,37,
4,7,16,1977,7,DM,M,36,
5,7,16,1977,3,DM,M,35,
6,7,16,1977,1,PF,M,14,
7,7,16,1977,2,PE,F,,
```

Load the new surveys data like before, and check what type of data we find in the `weight` column:

``` r
cats <- read.csv(file="surveys.csv")
typeof(data$weight)
```

    ## [1] "integer"

What happened? When R reads a csv file into one of these tables, it insists that everything in a column be the same basic type; if it can't understand *everything* in the column as a double, then *nobody* in the column gets to be a double. The table that R loaded our cats data into is something called a *data.frame*, and it is our first example of something called a *data structure* - that is, a structure which R knows how to build out of the basic data types.

We can see that it is a *data.frame* by calling the `class` function on it:

``` r
class(data)
```

    ## [1] "data.frame"

In order to successfully use our data in R, we need to understand what the basic data structures are, and how they behave. For now, let's remove that extra line from our cats data and reload it, while we investigate this behavior further:

surveys.csv:

    record_id,month,day,year,plot_id,species_id,sex,hindfoot_length,weight
    1,7,16,1977,2,NL,M,32,
    2,7,16,1977,3,NL,M,33,
    3,7,16,1977,2,DM,F,37,
    4,7,16,1977,7,DM,M,36,
    5,7,16,1977,3,DM,M,35,
    6,7,16,1977,1,PF,M,14,
    7,7,16,1977,2,PE,F,,

Selecting data using indices and sequences
------------------------------------------

When analyzing data, we often want to **partition the data so that we are only working with selected columns or rows.** A data frame or data matrix is simply a collection of vectors combined together. So let's begin with vectors and how to access different elements, and then extend those concepts to dataframes.
