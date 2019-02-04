4-datastructures-factors
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

### Factors

> Objectives
> ----------
>
> -   Understand how to represent catagorical data in R
> -   Know the difference between ordered and unordered factors
> -   Be aware of some of the problems encountered when using factors

This section is modeled after the [datacarpentry lessons](http://datacarpentry.org).

Factors are used to represent categorical data. Factors can be ordered or unordered and are an important class for statistical analysis and for plotting.

Factors are stored as integers, and have labels associated with these unique integers. While factors look (and often behave) like character vectors, they are actually integers under the hood, and you need to be careful when treating them like strings.

Once created, factors can only contain a pre-defined set values, known as *levels*. By default, R always sorts *levels* in alphabetical order. For instance, if you have a factor with 2 levels:

Tip
---

> The `factor()` command is used to create and modify factors in R

``` r
sex <- factor(c("male", "female", "female", "male"))
```

R will assign `1` to the level `"female"` and `2` to the level `"male"` (because `f` comes before `m`, even though the first element in this vector is `"male"`). You can check this by using the function `levels()`, and check the number of levels using `nlevels()`: \*\*\* Now R has noticed that there are three possible categories in our data - but it also did something surprising; instead of printing out the strings we gave it, we got a bunch of numbers instead. R has replaced our human-readable categories with numbered indices under the hood, this is necessary as many statistical calculations utilise such numerical representations for categorical data:

``` r
levels(sex)
```

    ## [1] "female" "male"

``` r
nlevels(sex)
```

    ## [1] 2

``` r
typeof(sex)
```

    ## [1] "integer"

Sometimes, the order of the factors does not matter, other times you might want to specify the order because it is meaningful (e.g., "low", "medium", "high") or it is required by particular type of analysis. Additionally, specifying the order of the levels allows us to compare levels:

``` r
food <- factor(c("low", "high", "medium", "high", "low", "medium", "high"))
levels(food)
```

    ## [1] "high"   "low"    "medium"

``` r
food <- factor(food, levels=c("low", "medium", "high"))
levels(food)
```

    ## [1] "low"    "medium" "high"

``` r
min(food) ## doesn't work
```

    ## Error in Summary.factor(structure(c(1L, 3L, 2L, 3L, 1L, 2L, 3L), .Label = c("low", : 'min' not meaningful for factors

``` r
food <- factor(food, levels=c("low", "medium", "high"), ordered=TRUE)
levels(food)
```

    ## [1] "low"    "medium" "high"

``` r
min(food) ## works!
```

    ## [1] low
    ## Levels: low < medium < high

In R's memory, these factors are represented by numbers (1, 2, 3). They are better than using simple integer labels because factors are self describing: `"low"`, `"medium"`, and `"high"`" is more descriptive than `1`, `2`, `3`. Which is low? You wouldn't be able to tell with just integer data. Factors have this information built in. It is particularly helpful when there are many levels (like the subjects in our example data set).

Challenge - Representing data in R
----------------------------------

> You have a vector representing levels of exercise undertaken by 5 subjects
>
> **"l","n","n","i","l"** ; n=none, l=light, i=intense
>
> What is the best way to represent this in R?
>
> 1.  exercise&lt;-c("l","n","n","i","l")
>
> 2.  exercise&lt;-factor(c("l","n","n","i","l"), ordered=TRUE)
>
> 3.  exercise&lt;-factor(c("l","n","n","i","l"), levels=c("n","l","i"), ordered=FALSE)
>
> 4.  exercise&lt;-factor(c("l","n","n","i","l"), levels=c("n","l","i"), ordered=TRUE)
>
### Converting factors

Converting from a factor to a number can cause problems:

``` r
f<-factor(c(3.4, 1.2, 5))
as.numeric(f)
```

    ## [1] 2 1 3

This does not behave as expected (and there is no warning).

The recommended way is to use the integer vector to index the factor levels:

``` r
levels(f)[f]
```

    ## [1] "3.4" "1.2" "5"

This returns a character vector, the `as.numeric()` function is still required to convert the values to the proper type (numeric).

``` r
f<-levels(f)[f]
f<-as.numeric(f)
```

### Using factors

Lets load our example data:

``` r
data<-read.csv(file='surveys.csv', stringsAsFactors=TRUE)
```

Tip
---

> `stringsAsFactors=TRUE` is the default behaviour for R. We could leave this argument out. It is included here for clarity.

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

Notice the first 3 columns have been converted to factors. These values were text in the data file so R automatically interpreted them as catagorical variables.

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

Notice the `summary()` function handles factors differently to numbers (and strings), the occurence counts for each value is often more useful information.

### Key Points

-   Factors are used to represent catagorical data
-   Factors can be *ordered* or *unordered*
-   Some R functions have special methods for handling functions
