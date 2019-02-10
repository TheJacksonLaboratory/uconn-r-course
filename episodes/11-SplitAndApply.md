-   Learn that functions are also objects and can be passed around as a
    variable
-   Learn to use *apply* to apply functions on lists of data

Split and Apply
===============

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
for each species, and then call mean(). This will work (and you probably
should try it out).

    surveys <- read.csv("surveys.csv")

    weight_DM <- surveys[surveys$species_id == "DM",]$weight
    mean(weight_DM, na.rm = TRUE)

    ## [1] 43.15786

    species_names <- unique(surveys$species_id)
    for (species in species_names) {
      weights <- surveys[surveys$species_id == species,]$weight
      print(species)
      print(mean(weights, na.rm = TRUE))
    }

    ## [1] "NL"
    ## [1] 159.2457
    ## [1] "DM"
    ## [1] 43.15786
    ## [1] "PF"
    ## [1] 7.923127
    ## [1] "PE"
    ## [1] 21.58651
    ## [1] "DS"
    ## [1] 120.1305
    ## [1] "PP"
    ## [1] 17.17394
    ## [1] "SH"
    ## [1] 73.14894
    ## [1] "OT"
    ## [1] 24.23056
    ## [1] "DO"
    ## [1] 48.87052
    ## [1] "OX"
    ## [1] 21
    ## [1] "SS"
    ## [1] 93.5
    ## [1] "OL"
    ## [1] 31.57526
    ## [1] "RM"
    ## [1] 10.58501
    ## [1] ""
    ## [1] NaN
    ## [1] "SA"
    ## [1] NaN
    ## [1] "PM"
    ## [1] 21.36416
    ## [1] "AH"
    ## [1] NaN
    ## [1] "DX"
    ## [1] NaN
    ## [1] "AB"
    ## [1] NaN
    ## [1] "CB"
    ## [1] NaN
    ## [1] "CM"
    ## [1] NaN
    ## [1] "CQ"
    ## [1] NaN
    ## [1] "RF"
    ## [1] 13.38667
    ## [1] "PC"
    ## [1] NaN
    ## [1] "PG"
    ## [1] NaN
    ## [1] "PH"
    ## [1] 31.06452
    ## [1] "PU"
    ## [1] NaN
    ## [1] "CV"
    ## [1] NaN
    ## [1] "UR"
    ## [1] NaN
    ## [1] "UP"
    ## [1] NaN
    ## [1] "ZL"
    ## [1] NaN
    ## [1] "UL"
    ## [1] NaN
    ## [1] "CS"
    ## [1] NaN
    ## [1] "SC"
    ## [1] NaN
    ## [1] "BA"
    ## [1] 8.6
    ## [1] "SF"
    ## [1] 58.87805
    ## [1] "RO"
    ## [1] 10.25
    ## [1] "AS"
    ## [1] NaN
    ## [1] "SO"
    ## [1] 55.41463
    ## [1] "PI"
    ## [1] 19.25
    ## [1] "ST"
    ## [1] NaN
    ## [1] "CU"
    ## [1] NaN
    ## [1] "SU"
    ## [1] NaN
    ## [1] "RX"
    ## [1] 15.5
    ## [1] "PB"
    ## [1] 31.73594
    ## [1] "PL"
    ## [1] 19.13889
    ## [1] "PX"
    ## [1] 19
    ## [1] "CT"
    ## [1] NaN
    ## [1] "US"
    ## [1] NaN

How to split *weight* according to *species\_id*?

    w <- split(surveys$weight, surveys$species_id)

passing functions as arguments
------------------------------

    - apply     Apply Functions Over Array Margins
    - lapply    Apply a Function over a List or Vector (returns list)
    - sapply    Apply a Function over a List or Vector (returns vector)
    - mapply    Apply a Function to Multiple List or Vector Arguments
    - tapply    Apply a Function Over a Ragged Array
    - vapply    Apply a Function over a List or Vector (returns vector with pre-specified type of return value)
    - rapply    Recursively Apply a Function to a List

    sapply(w, sum, na.rm = TRUE)

    ##            AB     AH     AS     BA     CB     CM     CQ     CS     CT 
    ##      0      0      0      0    387      0      0      0      0      0 
    ##     CU     CV     DM     DO     DS     DX     NL     OL     OT     OX 
    ##      0      0 442886 141920 281586      0 183451  30628  52338    126 
    ##     PB     PC     PE     PF     PG     PH     PI     PL     PM     PP 
    ##  89178      0  27199  12265      0    963    154    689  18715  51934 
    ##     PU     PX     RF     RM     RO     RX     SA     SC     SF     SH 
    ##      0     38   1004  26833     82     31      0      0   2414  10314 
    ##     SO     SS     ST     SU     UL     UP     UR     US     ZL 
    ##   2272    187      0      0      0      0      0      0      0
