Goals - Learn that functions are also objects and can be passed around
as an argument - Learn to use *apply* to apply functions on lists of
data

Some of the following materials are adapted from
[swcarpentry](https://github.com/swcarpentry/r-novice-gapminder) and
[hbctraining](https://github.com/hbctraining/Intro-to-R)

Summary statistics
------------------

In this section, we will introduce the *split* function that is very
useful when combined with *apply* for summary statistics.

Question: What is the average weight of each species in the surveys
data? We can ask many similar questions that appear extremely
frequently: What is the total weight of all rodents captured at each
plot? What is the max weight of rodents with species\_id "AB" captured
during each year?

The solutions to the above questions are almost identical: \* choose a
subset of the original dataset for the question and then \* call an
appropriate function.

Intuitively, you might want to write a for loop to select the weights
for each species, and then call mean(). This will work and you should
try it out.

    surveys <- read.csv("../surveys.csv")

    weight_DM <- surveys[surveys$species_id == "DM",]$weight
    mean(weight_DM, na.rm = TRUE)

    ## [1] 43.15786

    species_names <- unique(surveys$species_id)
    m <- vector(mode = "numeric", length = length(species_names))
    for (i in 1:length(species_names)) {
      weights <- surveys[surveys$species_id == species_names[i],]$weight
      m[i] <- mean(weights, na.rm = TRUE)
    }
    names(m) <- species_names
    m

    ##         NL         DM         PF         PE         DS         PP 
    ## 159.245660  43.157864   7.923127  21.586508 120.130546  17.173942 
    ##         SH         OT         DO         OX         SS         OL 
    ##  73.148936  24.230556  48.870523  21.000000  93.500000  31.575258 
    ##         RM                    SA         PM         AH         DX 
    ##  10.585010        NaN        NaN  21.364155        NaN        NaN 
    ##         AB         CB         CM         CQ         RF         PC 
    ##        NaN        NaN        NaN        NaN  13.386667        NaN 
    ##         PG         PH         PU         CV         UR         UP 
    ##        NaN  31.064516        NaN        NaN        NaN        NaN 
    ##         ZL         UL         CS         SC         BA         SF 
    ##        NaN        NaN        NaN        NaN   8.600000  58.878049 
    ##         RO         AS         SO         PI         ST         CU 
    ##  10.250000        NaN  55.414634  19.250000        NaN        NaN 
    ##         SU         RX         PB         PL         PX         CT 
    ##        NaN  15.500000  31.735943  19.138889  19.000000        NaN 
    ##         US 
    ##        NaN

There is a more efficient way: using **split** and **apply**.

Split
-----

How to split *weight* according to *species\_id*?

    w <- split(surveys$weight, surveys$species_id)

Apply: passing functions as arguments
-------------------------------------

    - apply     Apply Functions Over Array Margins
    - lapply    Apply a Function over a List or Vector (returns list)
    - sapply    Apply a Function over a List or Vector (returns vector)
    - mapply    Apply a Function to Multiple List or Vector Arguments
    - tapply    Apply a Function Over a Ragged Array
    - vapply    Apply a Function over a List or Vector (returns vector with pre-specified type of return value)
    - rapply    Recursively Apply a Function to a List

The first argument is a list of objects, the second argument is the name
of a function, followed by arguments for the second argument.

    sapply(w, mean, na.rm = TRUE)

    ##                    AB         AH         AS         BA         CB 
    ##        NaN        NaN        NaN        NaN   8.600000        NaN 
    ##         CM         CQ         CS         CT         CU         CV 
    ##        NaN        NaN        NaN        NaN        NaN        NaN 
    ##         DM         DO         DS         DX         NL         OL 
    ##  43.157864  48.870523 120.130546        NaN 159.245660  31.575258 
    ##         OT         OX         PB         PC         PE         PF 
    ##  24.230556  21.000000  31.735943        NaN  21.586508   7.923127 
    ##         PG         PH         PI         PL         PM         PP 
    ##        NaN  31.064516  19.250000  19.138889  21.364155  17.173942 
    ##         PU         PX         RF         RM         RO         RX 
    ##        NaN  19.000000  13.386667  10.585010  10.250000  15.500000 
    ##         SA         SC         SF         SH         SO         SS 
    ##        NaN        NaN  58.878049  73.148936  55.414634  93.500000 
    ##         ST         SU         UL         UP         UR         US 
    ##        NaN        NaN        NaN        NaN        NaN        NaN 
    ##         ZL 
    ##        NaN

There are many R packages that simplify various **apply** calls, a
particularly good one being "dplyr" by Hadley Wickham (in the
"tidyverse" collection). While learning the functions in base R is still
helpful for one to learn the concepts, those packages are more commonly
used in practice.

Exercise
--------

1.  Run the following codes, think about what they are trying
    to achieve.

<!-- -->

    # a)
    w <- split(surveys$weight, as.factor(surveys$year))
    sapply(w, mean, na.rm = TRUE)

    ##     1977     1978     1979     1980     1981     1982     1983     1984 
    ## 46.65038 67.91129 63.39028 62.44875 65.84389 53.76589 55.10283 50.95568 
    ##     1985     1986     1987     1988     1989     1990     1991     1992 
    ## 46.69457 55.05393 49.44364 45.05576 35.75469 35.48304 32.04003 33.32816 
    ##     1993     1994     1995     1996     1997     1998     1999     2000 
    ## 34.27103 34.54659 29.53039 28.20717 31.76189 34.82652 36.50326 32.39459 
    ##     2001     2002 
    ## 36.46953 35.64155

    # b)
    w <- split(surveys$weight, as.factor(surveys$plot_id))
    sapply(w, sum, na.rm = TRUE)

    ##      1      2      3      4      5      6      7      8      9     10 
    ##  98619 108370  55839  89434  44715  53749  13183  85057  93144   5173 
    ##     11     12     13     14     15     16     17     18     19     20 
    ##  77909 109832  55451  79967  23500  11801  90655  54048  22878  59469 
    ##     21     22     23     24 
    ##  25342  70282   7245  41932

    # c)
    sapply(split(surveys, surveys$species_id), nrow)

    ##          AB    AH    AS    BA    CB    CM    CQ    CS    CT    CU    CV 
    ##   763   303   437     2    46    50    13    16     1     1     1     1 
    ##    DM    DO    DS    DX    NL    OL    OT    OX    PB    PC    PE    PF 
    ## 10596  3027  2504    40  1252  1006  2249    12  2891    39  1299  1597 
    ##    PG    PH    PI    PL    PM    PP    PU    PX    RF    RM    RO    RX 
    ##     8    32     9    36   899  3123     5     6    75  2609     8     2 
    ##    SA    SC    SF    SH    SO    SS    ST    SU    UL    UP    UR    US 
    ##    75     1    43   147    43   248     1     5     4     8    10     4 
    ##    ZL 
    ##     2

Solutions a) average weights of all animals captured during each year b)
total weight of all animals captured at each plot c) number of captured
animals for each species
