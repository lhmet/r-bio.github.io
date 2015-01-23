---
layout: page
title: 02 -- `data.frame` and other data structures
time: 2 hours
---


# Presentation of the Survey Data



We are studying the species and weight of animals caught in plots in our study
area.  The dataset is stored as a `.csv` file: each row holds information for a
single animal, and the columns represent `survey_id` , `month`, `day`, `year`,
`plot`, `species` (a 2 letter code, see the `species.csv` file for
correspondance), `sex` ("M" for males and "F" for females), `wgt` (the weight in
grams).

The first few rows of the survey dataset look like this:

    "63","8","19","1977","3","DM","M","40"
    "64","8","19","1977","7","DM","M","48"
    "65","8","19","1977","4","DM","F","29"
    "66","8","19","1977","4","DM","F","46"
    "67","8","19","1977","7","DM","M","36"

To load our survey data, we need to locate the `surveys.csv` file. We will use
`read.csv()` to load into memory (as a `data.frame`) the content of the CSV
file.



```r
download.file("http://r-bio.github.io/data/surveys.csv", "data/surveys.csv")
surveys <- read.csv('data/surveys.csv')
```

<!--- this chunk if for internal use so code in this lesson can be evaluated --->


__At this point, make sure all participants have the data loaded__

This statement doesn't produce any output because assignment doesn't display
anything. If we want to check that our data has been loaded, we can print the
variable's value: `surveys`

Wow... that was a lot of output. At least it means the data loaded
properly. Let's check the top (the first 6 lines) of this `data.frame` using the
function `head()`:


```r
head(surveys)
```

```
##   record_id month day year plot species sex wgt
## 1         1     7  16 1977    2      NL   M  NA
## 2         2     7  16 1977    3      NL   M  NA
## 3         3     7  16 1977    2      DM   F  NA
## 4         4     7  16 1977    7      DM   M  NA
## 5         5     7  16 1977    3      DM   M  NA
## 6         6     7  16 1977    1      PF   M  NA
```

Let's now check the __str__ucture of this `data.frame` in more details with the
function `str()`:


```r
str(surveys)
```

```
## 'data.frame':	35549 obs. of  8 variables:
##  $ record_id: int  1 2 3 4 5 6 7 8 9 10 ...
##  $ month    : int  7 7 7 7 7 7 7 7 7 7 ...
##  $ day      : int  16 16 16 16 16 16 16 16 16 16 ...
##  $ year     : int  1977 1977 1977 1977 1977 1977 1977 1977 1977 1977 ...
##  $ plot     : int  2 3 2 7 3 1 2 1 1 6 ...
##  $ species  : Factor w/ 49 levels "","AB","AH","AS",..: 17 17 13 13 13 24 23 13 13 24 ...
##  $ sex      : Factor w/ 6 levels "","F","M","P",..: 3 3 2 3 3 3 2 3 2 2 ...
##  $ wgt      : int  NA NA NA NA NA NA NA NA NA NA ...
```

__Also, show how to get this information from the "Environment" tab in RStudio.__


### Challenge

Based on the output of `str(surveys)`, can you answer the following questions?

* What is the class of the object `surveys`?
* How many rows and how many columns are in this object?
* How many species have been recorded during these surveys?



As you can see, the columns `species` and `sex` are of a special class called
`factor`. Before we learn more about the `data.frame` class, we are going to
talk about factors. They are very useful but not necessarily intuitive, and
therefore require some attention.


## Factors



Factors are used to represent categorical data. Factors can be ordered or
unordered and are an important class for statistical analysis and for plotting.

Factors are stored as integers, and have labels associated with these unique
integers. While factors look (and often behave) like character vectors, they are
actually integers under the hood, and you need to be careful when treating them
like strings.

Once created, factors can only contain a pre-defined set values, known as
*levels*. By default, R always sorts *levels* in alphabetical order. For
instance, if you have a factor with 2 levels:


```r
sex <- factor(c("male", "female", "female", "male"))
```

R will assign `1` to the level `"female"` and `2` to the level `"male"` (because
`f` comes before `m`, even though the first element in this vector is
`"male"`). You can check this by using the function `levels()`, and check the
number of levels using `nlevels()`:


```r
levels(sex)
```

```
## [1] "female" "male"
```

```r
nlevels(sex)
```

```
## [1] 2
```

Sometimes, the order of the factors does not matter, other times you might want
to specify the order because it is meaningful (e.g., "low", "medium", "high") or
it is required by particular type of analysis. Additionally, specifying the
order of the levels allows to compare levels:


```r
food <- factor(c("low", "high", "medium", "high", "low", "medium", "high"))
levels(food)
```

```
## [1] "high"   "low"    "medium"
```

```r
food <- factor(food, levels=c("low", "medium", "high"))
levels(food)
```

```
## [1] "low"    "medium" "high"
```

```r
min(food) ## doesn't work
```

```
## Error in Summary.factor(structure(c(1L, 3L, 2L, 3L, 1L, 2L, 3L), .Label = c("low", : 'min' not meaningful for factors
```

```r
food <- factor(food, levels=c("low", "medium", "high"), ordered=TRUE)
levels(food)
```

```
## [1] "low"    "medium" "high"
```

```r
min(food) ## works!
```

```
## [1] low
## Levels: low < medium < high
```

In R's memory, these factors are represented by numbers (1, 2, 3). They are
better than using simple integer labels because factors are self describing:
`"low"`, `"medium"`, and `"high"`" is more descriptive than `1`, `2`, `3`. Which
is low?  You wouldn't be able to tell with just integer data. Factors have this
information built in. It is particularly helpful when there are many levels
(like the species in our example data set).

### Converting factors

If you need to convert a factor to a character vector, simply use
`as.character(x)`.

Converting a factor to a numeric vector is however a little trickier, and you
have to go via a character vector. Compare:


```r
f <- factor(c(1, 5, 10, 2))
as.numeric(f)               ## wrong! and there is no warning...
```

```
## [1] 1 3 4 2
```

```r
as.numeric(as.character(f)) ## works...
```

```
## [1]  1  5 10  2
```

```r
as.numeric(levels(f))[f]    ## The recommended way.
```

```
## [1]  1  5 10  2
```

### Challenge

The function `table()` tabulates observations and can be used to create
bar plots quickly. For instance:


```r
## Question: How can you recreate this plot but by having "control"
## being listed last instead of first?
exprmt <- factor(c("treat1", "treat2", "treat1", "treat3", "treat1", "control",
                   "treat1", "treat2", "treat3"))
table(exprmt)
```

```
## exprmt
## control  treat1  treat2  treat3 
##       1       4       2       2
```

```r
barplot(table(exprmt))
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-1.png) 

# Subsetting data



In particular for larger datasets, it can be tricky to remember the column
number that corresponds to a particular variable. (Are species names in column 5
or 7? oh, right... they are in column 6). In some cases, in which column the
variable will be can change if the script you are using adds or removes
columns. It's therefore often better to use column names to refer to a
particular variable, and it makes your code easier to read and your intentions
clearer.

You can do operations on a particular column, by selecting it using the `$`
sign. In this case, the entire column is a vector. For instance, to extract all
the weights from our datasets, we can use: `surveys$wgt`. You can use
`names(surveys)` or `colnames(surveys)` to remind yourself of the column names.

In some cases, you may way to select more than one column. You can do this using
the square brackets: `surveys[, c("wgt", "sex")]`.

When analyzing data, though, we often want to look at partial statistics, such
as the maximum value of a variable per species or the average value per plot.

One way to do this is to select the data we want, and create a new temporary
array, using the `subset()` function. For instance, if we just want to look at
the animals of the species "DO":


```r
surveys_DO <- subset(surveys, species == "DO")
```

### Challenge

1. What does the following do?



1. Use the function `subset` twice to create a `data.frame` that contains all
individuals of the species "DM" that were collected in 2002.
  * How many individuals of the species "DM" were collected in 2002?



## Adding a column to our dataset



Sometimes, you may have to add a new column to your dataset that represents a
new variable. You can add columns to a `data.frame` using the function `cbind()`
(__c__olumn __bind__). Beware, the additional column must have the same number
of elements as there are rows in the `data.frame`.

In our survey dataset, the species are represented by a 2-letter code (e.g.,
"AB"), however, we would like to include the species name. The correspondance
between the 2 letter code and the names are in the file `species.csv`. In this
file, one column includes the genus and another includes the species. First, we
are going to import this file in memory:


```
## Warning in file(file, "rt"): cannot open file
## '../../data/biology/species.csv': No such file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```


```r
species <- read.csv("data/species.csv")
```

We are then going to use the function `match()` to create a vector that contains
the genus names for all our observations. The function `match()` takes at least
2 arguments: the values to be matched (in our case the 2 letter code from the
`surveys` data frame held in the column `species`), and the table that contains
the values to be matched against (in our case the 2 letter code in the `species`
data frame held in the column `species_id`). The function returns the position
of the matches in the table, and this can be used to retrieve the genus names:


```r
surveys_spid_index <- match(surveys$species, species$species_id)
```

```
## Error in match(surveys$species, species$species_id): object 'species' not found
```

```r
surveys_genera <- species$genus[surveys_spid_index]
```

```
## Error in eval(expr, envir, enclos): object 'species' not found
```

Now that we have our vector of genus names, we can add it as a new column to our
`surveys` object:


```r
surveys <- cbind(surveys, genus=surveys_genera)
```

```
## Error in cbind(surveys, genus = surveys_genera): object 'surveys_genera' not found
```

### Challenge

Use the same approach to also include the species names in the `surveys` data
frame.





```
## Error in eval(expr, envir, enclos): object 'species' not found
```

```
## Error in cbind(surveys, species_name = surveys_species): object 'surveys_species' not found
```


```r
## and check out the result
head(surveys)
```

```
##   record_id month day year plot species sex wgt
## 1         1     7  16 1977    2      NL   M  NA
## 2         2     7  16 1977    3      NL   M  NA
## 3         3     7  16 1977    2      DM   F  NA
## 4         4     7  16 1977    7      DM   M  NA
## 5         5     7  16 1977    3      DM   M  NA
## 6         6     7  16 1977    1      PF   M  NA
```

<!--- should we cover merge()? --->

# Adding rows

<!--- Even if this is not optimal, using this approach requires to cover less   -->
<!--- material such as logical operations on vectors. Depending on how fast the -->
<!--- group moves, it might be better to show the correct way.                  -->



Let's create a `data.frame` that contains the information only for the species
"DO" and "DM". We know how to create the data set for each species with the
function `subset()`:


```r
surveys_DO <- subset(surveys, species == "DO")
surveys_DM <- subset(surveys, species == "DM")
```

Similarly to `cbind()` for columns, there is a function `rbind()` (__r__ow
__bind__) that puts together two `data.frame`. With `rbind()` the number of
columns and their names must be identical between the two objects:


```r
surveys_DO_DM <- rbind(surveys_DO, surveys_DM)
```

### Challenge

Using a similar approach, construct a new `data.frame` that only includes data
for the years 2000 and 2001.



# Removing columns



Just like you can select columns by their positions in the `data.frame` or by
their names, you can remove them similarly.

To remove it by column number:


```r
surveys_noDate <- surveys[, -c(2:4)]
colnames(surveys)
```

```
## [1] "record_id" "month"     "day"       "year"      "plot"      "species"  
## [7] "sex"       "wgt"
```

```r
colnames(surveys_noDate)
```

```
## [1] "record_id" "plot"      "species"   "sex"       "wgt"
```

The easiest way to remove by name is to use the `subset()` function. This time
we need to specify explicitly the argument `select` as the default is to subset
on rows (as above). The minus sign indicates the names of the columns to remove
(note that the column names should not be quoted):


```r
surveys_noDate2 <- subset(surveys, select=-c(month, day, year))
colnames(surveys_noDate2)
```

```
## [1] "record_id" "plot"      "species"   "sex"       "wgt"
```

# Removing rows



Typically rows are not associated with names, so to remove them from the
`data.frame`, you can do:


```r
surveys_missingRows <- surveys[-c(10, 50:70), ] # removing rows 10, and 50 to 70
```


# Calculating statistics



Let's get a closer look at our data. For instance, we might want to know how
many animals we trapped in each plot, or how many of each species were caught.

To get a `vector` of all the species, we are going to use the `unique()`
function that tells us the unique values in a given vector:


```r
unique(surveys$species)
```

```
##  [1] NL DM PF PE DS PP SH OT DO OX SS OL RM    SA PM AH DX AB CB CM CQ RF
## [24] PC PG PH PU CV UR UP ZL UL CS SC BA SF RO AS SO PI ST CU SU RX PB PL
## [47] PX CT US
## 49 Levels:  AB AH AS BA CB CM CQ CS CT CU CV DM DO DS DX NL OL OT ... ZL
```

The function `table()`, tells us how many of each species we have:


```r
table(surveys$species)
```

```
## 
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
```

R has a lot of built in statistical functions, like `mean()`, `median()`,
`max()`, `min()`. Let's start by calculating the average weight of all the
animals using the function `mean()`:


```r
mean(surveys$wgt)
```

```
## [1] NA
```

Hmm, we just get `NA`. That's because we don't have the weight for every animal
and missing data is recorded as `NA`. By default, all R functions operating on a
vector that contains missing data will return NA. It's a way to make sure that
users know they have missing data, and make a conscious decision on how to deal
with it.

When dealing with simple statistics like the mean, the easiest way to ignore
`NA` (the missing data) is to use `na.rm=TRUE` (`rm` stands for remove):


```r
mean(surveys$wgt, na.rm=TRUE)
```

```
## [1] 42.67243
```

In some cases, it might be useful to remove the missing data from the
vector. For this purpose, R comes with the function `na.omit`:


```r
wgt_noNA <- na.omit(surveys$wgt)
```

For some applications, it's useful to keep all observations, for others, it
might be best to remove all observations that contain missing data. The function
`complete.cases()` removes any rows that contain at least one missing
observation:


```r
surveys_complete <- surveys[complete.cases(surveys), ]
```

<!--- need to cover negation, and vector operations for this...
If you want to remove only the observations that are missing data for one
variable, you can use the function `is.na()`. For instance, to create a new
dataset that only contains individuals that have been weighted:


```r
surveys_with_weights <- surveys[!is.na(surveys$weight), ]
```

```
## Warning in is.na(surveys$weight): is.na() applied to non-(list or vector)
## of type 'NULL'
```
--->

### Challenge

1. To determine the number of elements found in a vector, we can use
use the function `length()` (e.g., `length(surveys$wgt)`). Using `length()`, how
many animals have not had their weights recorded?

1. What is the median weight for the males?

1. What is the range (minimum and maximum) weight?

1. Bonus question: what is the standard error for the weight? (hints: there is
   no built-in function to compute standard errors, and the function for the
   square root is `sqrt()`).



# Statistics across factor levels



What if we want the maximum weight for all animals, or the average for each
plot?

R comes with convenient functions to do this kind of operations, functions in
the `apply` family.

For instance, `tapply()` allows us to repeat a function across each level of a
factor. The format is:


```r
tapply(columns_to_do_the_calculations_on, factor_to_sort_on, function)
```

If we want to calculate the mean for each species (using the complete dataset):


```r
tapply(surveys_complete$wgt, surveys_complete$species, mean)
```

```
##                    AB         AH         AS         BA         CB 
##         NA         NA         NA         NA   8.600000         NA 
##         CM         CQ         CS         CT         CU         CV 
##         NA         NA         NA         NA         NA         NA 
##         DM         DO         DS         DX         NL         OL 
##  43.157864  48.870523 120.130546         NA 159.245660  31.575258 
##         OT         OX         PB         PC         PE         PF 
##  24.230556  21.000000  31.735943         NA  21.586508   7.923127 
##         PG         PH         PI         PL         PM         PP 
##         NA  31.064516  19.250000  19.138889  21.364155  17.173942 
##         PU         PX         RF         RM         RO         RX 
##         NA  19.000000  13.386667  10.585010  10.250000  15.500000 
##         SA         SC         SF         SH         SO         SS 
##         NA         NA  58.878049  73.148936  55.414634  93.500000 
##         ST         SU         UL         UP         UR         US 
##         NA         NA         NA         NA         NA         NA 
##         ZL 
##         NA
```

This produces some `NA` because R "remembers" all species that were found in the
original dataset, even if they didn't have any weight data associated with them
in the current dataset. To remove the `NA` and make things clearer, we can
redefine the levels for the factor "species" before calculating the means. Let's
also create an object to store these values:


```r
surveys_complete$species <- factor(surveys_complete$species)
species_mean <- tapply(surveys_complete$wgt, surveys_complete$species, mean)
```

### Challenge

1. Create new objects to store: the standard deviation, the maximum and minimum
   values for the weight of each species
1. How many species do you have these statistics for?
1. Create a new data frame (called `surveys_summary`) that contains as columns:
   * `species` the 2 letter code for the species names
   * `mean_wgt` the mean weight for each species
   * `sd_wgt` the standard deviation for each species
   * `min_wgt`  the minimum weight for each species
   * `max_wgt`  the maximum weight for each species



**Answers**


```r
species_max <- tapply(surveys_complete$wgt, surveys_complete$species, max)
species_min <- tapply(surveys_complete$wgt, surveys_complete$species, min)
species_sd <- tapply(surveys_complete$wgt, surveys_complete$species, sd)
nlevels(surveys_complete$species) # or length(species_mean)
```

```
## [1] 25
```

```r
surveys_summary <- data.frame(species=levels(surveys_complete$species),
                              mean_wgt=species_mean,
                              sd_wgt=species_sd,
                              min_wgt=species_min,
                              max_wgt=species_max)
```
