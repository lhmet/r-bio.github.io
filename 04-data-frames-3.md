---
layout: page
title: 04 -- Even more `data.frame`
---

# Quick review

## `[` and `[[`

### Challenges

* Given a vector of random numbers (picked from a normal distribution) `vec <- rnorm(100)`, retrieve the 5th, 10th and 15th elements from it.

* R has a special vector called `letters` that stores the letter of the alphabet. Use it in combination with the function `rep()` to create a vector of 100 that contains only `a` and `e`.

* Starting with `vec <- 1:100`, and using the function `seq()` create a vector that doesn't contain the multiples of 5 (i.e., 5, 10, ... 100).


# Removing rows



Typically, rows are not associated with names, so to remove them from the
`data.frame`, you can do:


```r
surveys <- read.csv(file="data/surveys.csv")
surveys_missingRows <- surveys[-c(10, 50:70), ] # removing rows 10, and 50 to 70
```


# Calculating statistics



Let's get a closer look at our data. For instance, we might want to know how
many animals we trapped in each plot, or how many of each species were caught.

To get a `vector` of all the species, we are going to use the `unique()`
function that tells us the unique values in a given vector:


```r
unique(surveys$species_id)
```

```
##  [1] NL DM PF PE DS PP SH OT DO OX SS OL RM    SA PM AH DX AB CB CM CQ RF
## [24] PC PG PH PU CV UR UP ZL UL CS SC BA SF RO AS SO PI ST CU SU RX PB PL
## [47] PX CT US
## 49 Levels:  AB AH AS BA CB CM CQ CS CT CU CV DM DO DS DX NL OL OT ... ZL
```

The function `table()`, tells us how many of each species we have:


```r
table(surveys$species_id)
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
tapply(surveys_complete$wgt, surveys_complete$species_id, mean)
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
surveys_complete$species_id <- factor(surveys_complete$species_id)
species_mean <- tapply(surveys_complete$wgt, surveys_complete$species_id, mean)
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
species_max <- tapply(surveys_complete$wgt, surveys_complete$species_id, max)
species_min <- tapply(surveys_complete$wgt, surveys_complete$species_id, min)
species_sd <- tapply(surveys_complete$wgt, surveys_complete$species_id, sd)
nlevels(surveys_complete$species_id) # or length(species_mean)
```

```
## [1] 25
```

```r
surveys_summary <- data.frame(species=levels(surveys_complete$species_id),
                              mean_wgt=species_mean,
                              sd_wgt=species_sd,
                              min_wgt=species_min,
                              max_wgt=species_max)
```
