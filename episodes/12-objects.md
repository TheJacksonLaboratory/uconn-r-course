12-Objects
================
David Mellert
2/7/2019

General Object Concepts
-----------------------

In programming, **objects** are code/data structures that provide a level of abstraction for building your program. Programming objects are often used to model real world objects, like people or automobiles, but can also represent abstract concepts, like scientific results. An object can contain various types of data, called **attributes**, and have associated functions, called **methods**.

A **class** is a category of objects that share attributes (although not necessarily the same attribute values) and methods. An object is an **instance** of a class.

For example you might have a class that is 'automobile', which has the attributes 'make', 'model' and 'engine\_type' as well as the methods 'start' and 'rev\_engine'.

Then you might have two objects:

1.  Instance \#1:

-   class: automobile
-   make: Ford
-   model: Fusion
-   engine\_type: Hybrid
-   start( ): Engine turns on
-   rev\_engine( ): Makes a sound

1.  Instance \#2:

-   class: automobile
-   make: Bugatti
-   model: Veyron
-   engine\_type: V16
-   start( ): Engine turns on
-   rev\_engine( ): Makes a **much** cooler sound

Note how the two instances have the same class and the same attribute keys, but have different values for the attributes. Also note that they both have the method called 'start', which does the same thing in both instances, and 'rev\_engine', whose behavior is different between the two objects (undoubtedly because of the difference in the 'engine\_type' attribute!).

There are other interesting and important object oriented programming concepts that you might want to explore, such as **class inheritance**, but we will not get into those concepts here.

Objects in R
------------

First, we should mention that Hadley Wickham has an excellent [chapter on object oriented programming in R](https://adv-r.hadley.nz/oo.html) in his book [Advanced R](https://adv-r.hadley.nz/), which you are encouraged to check out! You might also refer to an older version of the same website [here](http://adv-r.had.co.nz/OO-essentials.html).

Objects are a bit confusing in R because there are multiple different object systems that all work a bit differently. These different systems can be broadly categorized into:

1.  S3/S4 -- These are the older object systems in R and are used throughout "Base R" and Bioconductor.
2.  Reference Classes -- Reference Classes (RC) are a more recent development in R. RC objects work fairly similarly to how objects work in other languages (e.g., Python), but you will not see these nearly as often as S3/S4 objects, so they will not be covered in this lesson.

S3 Objects
----------

S3 is the original object system in R, and it is very simple. S3 objects are basically lists, and their attributes are the named elements of the list. For example, we can create an automobile object by first creating a named list:

``` r
car <- list(make = 'Ford', model='Fusion', engine_type = 'Hybrid')
car
```

    ## $make
    ## [1] "Ford"
    ## 
    ## $model
    ## [1] "Fusion"
    ## 
    ## $engine_type
    ## [1] "Hybrid"

Now we can turn that `car` list into an object of class `automobile` with the `class()` function like so:

``` r
class(car) <- 'automobile'
car
```

    ## $make
    ## [1] "Ford"
    ## 
    ## $model
    ## [1] "Fusion"
    ## 
    ## $engine_type
    ## [1] "Hybrid"
    ## 
    ## attr(,"class")
    ## [1] "automobile"

You can see the class now shows up. You can also check the class of an object with the `class()` function:

``` r
class(car)
```

    ## [1] "automobile"

Nonetheless, `car` is still a list:

``` r
typeof(car)
```

    ## [1] "list"

As with any named list, you can use `$` to access attributes:

``` r
car$make
```

    ## [1] "Ford"

We can create a second instance called `car2`:

``` r
car2 <- list(make = 'Bugatti', model='Veyron', engine_type = 'V16')
class(car2) <- 'automobile'
car2
```

    ## $make
    ## [1] "Bugatti"
    ## 
    ## $model
    ## [1] "Veyron"
    ## 
    ## $engine_type
    ## [1] "V16"
    ## 
    ## attr(,"class")
    ## [1] "automobile"

And that's it! This system is very informal, and one has to be very careful when using it. For example, you could create a list that is very unlike a car and assign it the `automobile` class:

``` r
dog <- list(legs = 4, coat_color = 'brown', face = 'cute')
class(dog) <- 'automobile'
dog
```

    ## $legs
    ## [1] 4
    ## 
    ## $coat_color
    ## [1] "brown"
    ## 
    ## $face
    ## [1] "cute"
    ## 
    ## attr(,"class")
    ## [1] "automobile"

This could obviously cause problems, and may come as a surprise to programmers experienced with other languages that have more formal object systems. But this is the way the `base` and `stats` packages handle objects, so it is something we must deal with.

You'll notice that we didn't give our objects any methods. This is because, in S3, class methods don't 'belong' to the class (there is no true class definition for S3, anyway). Instead, methods are functions that belong to a special type of parent function called a **generic function**, (or 'generic', in R lingo). To help explore how generics work, lets go back to the `surveys.csv` data:

``` r
data <- read.csv('../surveys.csv') #your path may differ here!
class(data)
```

    ## [1] "data.frame"

Now lets look what happens when we use the `print()` function on a few things.

``` r
print('hello world')
```

    ## [1] "hello world"

``` r
print(car)
```

    ## $make
    ## [1] "Ford"
    ## 
    ## $model
    ## [1] "Fusion"
    ## 
    ## $engine_type
    ## [1] "Hybrid"
    ## 
    ## attr(,"class")
    ## [1] "automobile"

``` r
data_small <- data[1:10,] #subsetting to keep things tidy and readable
print(data_small) 
```

    ##    record_id month day year plot_id species_id sex hindfoot_length weight
    ## 1          1     7  16 1977       2         NL   M              32     NA
    ## 2          2     7  16 1977       3         NL   M              33     NA
    ## 3          3     7  16 1977       2         DM   F              37     NA
    ## 4          4     7  16 1977       7         DM   M              36     NA
    ## 5          5     7  16 1977       3         DM   M              35     NA
    ## 6          6     7  16 1977       1         PF   M              14     NA
    ## 7          7     7  16 1977       2         PE   F              NA     NA
    ## 8          8     7  16 1977       1         DM   M              37     NA
    ## 9          9     7  16 1977       1         DM   F              34     NA
    ## 10        10     7  16 1977       6         PF   F              20     NA

So you can use the same `print()` function on a regular string, on our `car` automobile object, and on our `data_small` data.frame object. But you might notice that `data_small` prints out in a nice way, whereas `car` just looks like the same old list we started with.

Now what does print do when we remove the `object`ness of `car` and `data_small` with `unclass()`?

``` r
car <- unclass(car)
print(car)
```

    ## $make
    ## [1] "Ford"
    ## 
    ## $model
    ## [1] "Fusion"
    ## 
    ## $engine_type
    ## [1] "Hybrid"

``` r
data_small <- unclass(data_small)
print(data_small)
```

    ## $record_id
    ##  [1]  1  2  3  4  5  6  7  8  9 10
    ## 
    ## $month
    ##  [1] 7 7 7 7 7 7 7 7 7 7
    ## 
    ## $day
    ##  [1] 16 16 16 16 16 16 16 16 16 16
    ## 
    ## $year
    ##  [1] 1977 1977 1977 1977 1977 1977 1977 1977 1977 1977
    ## 
    ## $plot_id
    ##  [1] 2 3 2 7 3 1 2 1 1 6
    ## 
    ## $species_id
    ##  [1] NL NL DM DM DM PF PE DM DM PF
    ## 49 Levels:  AB AH AS BA CB CM CQ CS CT CU CV DM DO DS DX NL OL OT ... ZL
    ## 
    ## $sex
    ##  [1] M M F M M M F M F F
    ## Levels:  F M
    ## 
    ## $hindfoot_length
    ##  [1] 32 33 37 36 35 14 NA 37 34 20
    ## 
    ## $weight
    ##  [1] NA NA NA NA NA NA NA NA NA NA
    ## 
    ## attr(,"row.names")
    ##  [1]  1  2  3  4  5  6  7  8  9 10

`car` didn't change at all, except for losing its class attribute, but now `data_small` is printing like a list! What happened?

`print()` is a generic function whose job is to find the appropriate method to use on an object. The `data.frame` class indeed has a `print.data.frame()` method that 'belongs' to `print()`. This can be explored with the `methods()` function:

``` r
methods(print)
```

    ##   [1] print.acf*                                        
    ##   [2] print.AES*                                        
    ##   [3] print.anova*                                      
    ##   [4] print.aov*                                        
    ##   [5] print.aovlist*                                    
    ##   [6] print.ar*                                         
    ##   [7] print.Arima*                                      
    ##   [8] print.arima0*                                     
    ##   [9] print.AsIs                                        
    ##  [10] print.aspell*                                     
    ##  [11] print.aspell_inspect_context*                     
    ##  [12] print.bibentry*                                   
    ##  [13] print.Bibtex*                                     
    ##  [14] print.browseVignettes*                            
    ##  [15] print.by                                          
    ##  [16] print.bytes*                                      
    ##  [17] print.changedFiles*                               
    ##  [18] print.check_code_usage_in_package*                
    ##  [19] print.check_compiled_code*                        
    ##  [20] print.check_demo_index*                           
    ##  [21] print.check_depdef*                               
    ##  [22] print.check_details*                              
    ##  [23] print.check_details_changes*                      
    ##  [24] print.check_doi_db*                               
    ##  [25] print.check_dotInternal*                          
    ##  [26] print.check_make_vars*                            
    ##  [27] print.check_nonAPI_calls*                         
    ##  [28] print.check_package_code_assign_to_globalenv*     
    ##  [29] print.check_package_code_attach*                  
    ##  [30] print.check_package_code_data_into_globalenv*     
    ##  [31] print.check_package_code_startup_functions*       
    ##  [32] print.check_package_code_syntax*                  
    ##  [33] print.check_package_code_unload_functions*        
    ##  [34] print.check_package_compact_datasets*             
    ##  [35] print.check_package_CRAN_incoming*                
    ##  [36] print.check_package_datasets*                     
    ##  [37] print.check_package_depends*                      
    ##  [38] print.check_package_description*                  
    ##  [39] print.check_package_description_encoding*         
    ##  [40] print.check_package_license*                      
    ##  [41] print.check_packages_in_dir*                      
    ##  [42] print.check_packages_used*                        
    ##  [43] print.check_po_files*                             
    ##  [44] print.check_pragmas*                              
    ##  [45] print.check_Rd_contents*                          
    ##  [46] print.check_Rd_line_widths*                       
    ##  [47] print.check_Rd_metadata*                          
    ##  [48] print.check_Rd_xrefs*                             
    ##  [49] print.check_RegSym_calls*                         
    ##  [50] print.check_so_symbols*                           
    ##  [51] print.check_T_and_F*                              
    ##  [52] print.check_url_db*                               
    ##  [53] print.check_vignette_index*                       
    ##  [54] print.checkDocFiles*                              
    ##  [55] print.checkDocStyle*                              
    ##  [56] print.checkFF*                                    
    ##  [57] print.checkRd*                                    
    ##  [58] print.checkReplaceFuns*                           
    ##  [59] print.checkS3methods*                             
    ##  [60] print.checkTnF*                                   
    ##  [61] print.checkVignettes*                             
    ##  [62] print.citation*                                   
    ##  [63] print.codoc*                                      
    ##  [64] print.codocClasses*                               
    ##  [65] print.codocData*                                  
    ##  [66] print.colorConverter*                             
    ##  [67] print.compactPDF*                                 
    ##  [68] print.condition                                   
    ##  [69] print.connection                                  
    ##  [70] print.CRAN_package_reverse_dependencies_and_views*
    ##  [71] print.data.frame                                  
    ##  [72] print.Date                                        
    ##  [73] print.default                                     
    ##  [74] print.dendrogram*                                 
    ##  [75] print.density*                                    
    ##  [76] print.difftime                                    
    ##  [77] print.dist*                                       
    ##  [78] print.Dlist                                       
    ##  [79] print.DLLInfo                                     
    ##  [80] print.DLLInfoList                                 
    ##  [81] print.DLLRegisteredRoutines                       
    ##  [82] print.dummy_coef*                                 
    ##  [83] print.dummy_coef_list*                            
    ##  [84] print.ecdf*                                       
    ##  [85] print.eigen                                       
    ##  [86] print.factanal*                                   
    ##  [87] print.factor                                      
    ##  [88] print.family*                                     
    ##  [89] print.fileSnapshot*                               
    ##  [90] print.findLineNumResult*                          
    ##  [91] print.formula*                                    
    ##  [92] print.fseq*                                       
    ##  [93] print.ftable*                                     
    ##  [94] print.function                                    
    ##  [95] print.getAnywhere*                                
    ##  [96] print.glm*                                        
    ##  [97] print.hclust*                                     
    ##  [98] print.help_files_with_topic*                      
    ##  [99] print.hexmode                                     
    ## [100] print.HoltWinters*                                
    ## [101] print.hsearch*                                    
    ## [102] print.hsearch_db*                                 
    ## [103] print.htest*                                      
    ## [104] print.html*                                       
    ## [105] print.html_dependency*                            
    ## [106] print.infl*                                       
    ## [107] print.integrate*                                  
    ## [108] print.isoreg*                                     
    ## [109] print.kmeans*                                     
    ## [110] print.knitr_kable*                                
    ## [111] print.Latex*                                      
    ## [112] print.LaTeX*                                      
    ## [113] print.libraryIQR                                  
    ## [114] print.listof                                      
    ## [115] print.lm*                                         
    ## [116] print.loadings*                                   
    ## [117] print.loess*                                      
    ## [118] print.logLik*                                     
    ## [119] print.ls_str*                                     
    ## [120] print.medpolish*                                  
    ## [121] print.MethodsFunction*                            
    ## [122] print.mtable*                                     
    ## [123] print.NativeRoutineList                           
    ## [124] print.news_db*                                    
    ## [125] print.nls*                                        
    ## [126] print.noquote                                     
    ## [127] print.numeric_version                             
    ## [128] print.object_size*                                
    ## [129] print.octmode                                     
    ## [130] print.packageDescription*                         
    ## [131] print.packageInfo                                 
    ## [132] print.packageIQR*                                 
    ## [133] print.packageStatus*                              
    ## [134] print.pairwise.htest*                             
    ## [135] print.PDF_Array*                                  
    ## [136] print.PDF_Dictionary*                             
    ## [137] print.pdf_doc*                                    
    ## [138] print.pdf_fonts*                                  
    ## [139] print.PDF_Indirect_Reference*                     
    ## [140] print.pdf_info*                                   
    ## [141] print.PDF_Keyword*                                
    ## [142] print.PDF_Name*                                   
    ## [143] print.PDF_Stream*                                 
    ## [144] print.PDF_String*                                 
    ## [145] print.person*                                     
    ## [146] print.POSIXct                                     
    ## [147] print.POSIXlt                                     
    ## [148] print.power.htest*                                
    ## [149] print.ppr*                                        
    ## [150] print.prcomp*                                     
    ## [151] print.princomp*                                   
    ## [152] print.proc_time                                   
    ## [153] print.raster*                                     
    ## [154] print.Rcpp_stack_trace*                           
    ## [155] print.Rd*                                         
    ## [156] print.recordedplot*                               
    ## [157] print.restart                                     
    ## [158] print.RGBcolorConverter*                          
    ## [159] print.rle                                         
    ## [160] print.roman*                                      
    ## [161] print.sessionInfo*                                
    ## [162] print.shiny.tag*                                  
    ## [163] print.shiny.tag.list*                             
    ## [164] print.simple.list                                 
    ## [165] print.smooth.spline*                              
    ## [166] print.socket*                                     
    ## [167] print.srcfile                                     
    ## [168] print.srcref                                      
    ## [169] print.stepfun*                                    
    ## [170] print.stl*                                        
    ## [171] print.StructTS*                                   
    ## [172] print.subdir_tests*                               
    ## [173] print.summarize_CRAN_check_status*                
    ## [174] print.summary.aov*                                
    ## [175] print.summary.aovlist*                            
    ## [176] print.summary.ecdf*                               
    ## [177] print.summary.glm*                                
    ## [178] print.summary.lm*                                 
    ## [179] print.summary.loess*                              
    ## [180] print.summary.manova*                             
    ## [181] print.summary.nls*                                
    ## [182] print.summary.packageStatus*                      
    ## [183] print.summary.ppr*                                
    ## [184] print.summary.prcomp*                             
    ## [185] print.summary.princomp*                           
    ## [186] print.summary.table                               
    ## [187] print.summary.warnings                            
    ## [188] print.summaryDefault                              
    ## [189] print.table                                       
    ## [190] print.tables_aov*                                 
    ## [191] print.terms*                                      
    ## [192] print.ts*                                         
    ## [193] print.tskernel*                                   
    ## [194] print.TukeyHSD*                                   
    ## [195] print.tukeyline*                                  
    ## [196] print.tukeysmooth*                                
    ## [197] print.undoc*                                      
    ## [198] print.vignette*                                   
    ## [199] print.warnings                                    
    ## [200] print.xfun_raw_string*                            
    ## [201] print.xfun_strict_list*                           
    ## [202] print.xgettext*                                   
    ## [203] print.xngettext*                                  
    ## [204] print.xtabs*                                      
    ## see '?methods' for accessing help and source code

``` r
methods(class='automobile')
```

    ## no methods found

``` r
methods(class='data.frame')
```

    ##  [1] [             [[            [[<-          [<-           $            
    ##  [6] $<-           aggregate     anyDuplicated as.data.frame as.list      
    ## [11] as.matrix     by            cbind         coerce        dim          
    ## [16] dimnames      dimnames<-    droplevels    duplicated    edit         
    ## [21] format        formula       head          initialize    is.na        
    ## [26] Math          merge         na.exclude    na.omit       Ops          
    ## [31] plot          print         prompt        rbind         row.names    
    ## [36] row.names<-   rowsum        show          slotsFromS3   split        
    ## [41] split<-       stack         str           subset        summary      
    ## [46] Summary       t             tail          transform     type.convert 
    ## [51] unique        unstack       within       
    ## see '?methods' for accessing help and source code

From this we can see that `print()` works generically with the `automobile` class because we have not defined a `print.automobile()` method. When you use `print()` with a `data.frame`, `print.data.frame()` is used. Also, we see that `data.frame` has many associated methods, like `merge()`, `Summary()`, and `plot()` (actually, the methods would be `merge.data.frame()`, `Summary.data.frame()`, and `plot.data.frame()`).

We could create a `print.automobile()` method like this:

``` r
#first define a function
print_car <- function(x){  #function has to take at least one argument, even if you don't use it
  paste('Vroooooom!')
}

#now assign the function to the appropriate method
print.automobile <- print_car
print(car2)
```

    ## [1] "Vroooooom!"

Keep in mind that we can easily change the class of any object:

``` r
car2 <- unclass(car2)
print(car2)
```

    ## $make
    ## [1] "Bugatti"
    ## 
    ## $model
    ## [1] "Veyron"
    ## 
    ## $engine_type
    ## [1] "V16"

``` r
class(car2) <- 'data.frame' #this doesn't look great, for obvious reasons!
print(car2)
```

    ## [1] make        model       engine_type
    ## <0 rows> (or 0-length row.names)

``` r
class(car2) <- 'automobile'
print(car2)
```

    ## [1] "Vroooooom!"

### Exercise 1

First, run the following line of code. This is to create a histogram of the hindfoot\_length vector in our data.frame:

``` r
hf_histogram <- hist(data$hindfoot_length, plot=FALSE) #creates a plot by default, turn it off for now
```

-   What is the type of `hf_histogram`? The class?

-   What are the attributes of `hf_histogram`?

-   What methods are specific to `hf_histogram`'s class? What are the generics for those methods?

S4 Objects
----------

The S4 object system is very similar to S3 in that the underlying data structure is a named list, where each named item becomes an attribute, and methods belong to generic functions. However S4 differs from S3 in that it has a formal system for class definitions, and attributes (or 'slots', in S4 terminology) are accessed with `@` instead of `$`. You also need to use a function to create instances of each class. This will either be a dedicated constructor function or `new( )`.

Whereas S3 objects are used throughout the R `base` and `stats` packages, you will see S4 used throughout Bioconductor and its associated packages.

Lets take a look at an S4 object example:

``` r
setClass('automobile2',
         slots= list(make = 'character', #Here, 'character' indicates the data type
                     model = 'character',
                     n_wheels = 'integer')) #Obviously, number of wheels must be an integer!

car3 <- new('automobile2', make = 'Audi', model = 'A7', n_wheels = as.integer(4))
car3@model #note the use of @ instead of $
```

    ## [1] "A7"

``` r
class(car3)
```

    ## [1] "automobile2"
    ## attr(,"package")
    ## [1] ".GlobalEnv"

``` r
typeof(car3)
```

    ## [1] "S4"

You can create methods like this:

``` r
setMethod('print',
          'automobile2',
          function(x){
            paste('This car has', x@n_wheels, 'wheels') #note the @
          }
)

print(car3)
```

    ## [1] "This car has 4 wheels"

``` r
car3 # notice that S4 objects don't use print() when you call them  
```

    ## An object of class "automobile2"
    ## Slot "make":
    ## [1] "Audi"
    ## 
    ## Slot "model":
    ## [1] "A7"
    ## 
    ## Slot "n_wheels":
    ## [1] 4

As you can probably guess, S4 objects are quite a bit 'safer' than S3 objects. For example, you can't create objects with attributes that don't match the class:

``` r
dog2 <- new('automobile2', legs = 4, coat_color = 'brown')
```

    ## Error in initialize(value, ...): invalid names for slots of class "automobile2": legs, coat_color

### Exercise 2

For this exercise, first run the following code (from the Example for mle{stats4}):

``` r
library(stats4)
y <- c(26, 17, 13, 12, 20, 5, 9, 8, 5, 4, 8)
nLL <- function(lambda) -sum(stats::dpois(y, lambda, log = TRUE))
fit0 <- mle(nLL, start = list(lambda = 5), nobs = NROW(y))
```

-   What is the type of `fit0`? The class?

-   What are the slots for `fit0`?

-   Which method is used when you call `fit0` directly?
