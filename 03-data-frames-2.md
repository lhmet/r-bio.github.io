---
layout: page
title: `data.frame` continued
---

# Getting back where we started

Back to directory with `data/` and `data/surveys.csv`


```
##   X record_id month day year plot species_id sex wgt
## 1 1         1     7  16 1977    2         NL   M  NA
## 2 2         2     7  16 1977    3         NL   M  NA
## 3 3         3     7  16 1977    2         DM   F  NA
## 4 4         4     7  16 1977    7         DM   M  NA
## 5 5         5     7  16 1977    3         DM   M  NA
## 6 6         6     7  16 1977    1         PF   M  NA
```

```
## 'data.frame':	35549 obs. of  9 variables:
##  $ X         : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ record_id : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ month     : int  7 7 7 7 7 7 7 7 7 7 ...
##  $ day       : int  16 16 16 16 16 16 16 16 16 16 ...
##  $ year      : int  1977 1977 1977 1977 1977 1977 1977 1977 1977 1977 ...
##  $ plot      : int  2 3 2 7 3 1 2 1 1 6 ...
##  $ species_id: chr  "NL" "NL" "DM" "DM" ...
##  $ sex       : chr  "M" "M" "F" "M" ...
##  $ wgt       : int  NA NA NA NA NA NA NA NA NA NA ...
```

# About the `data.frame` class



`data.frame` is the _de facto_ data structure for most tabular data and what we
use for statistics and plotting.

A `data.frame` is a collection of vectors of identical lengths. Each vector
represents a column, and each vector can be of a different data type (e.g.,
characters, integers, factors). The `str()` function is useful to inspect the
data types of the columns.

`data.frame` can be created by the functions `read.csv()` or `read.table()`, in
other words, when importing spreadsheets from your hard drive (or the web).

By default, `data.frame` convert (= coerce) columns that contain characters
(i.e., text) into the `factor` data type. Depending on what you want to do with
the data, you may want to keep these columns as `character`. To do so,
`read.csv()` and `read.table()` have an argument called `stringsAsFactors` which
can be set to `FALSE`:


```r
some_data <- read.csv("data/some_file.csv", stringsAsFactors=FALSE)
```

<!--- talk about colClasses argument?  --->

You can also create `data.frame` manually with the function `data.frame()`. This
function can also take the argument `stringsAsFactors`. Compare the output of
these examples:


```r
example_data <- data.frame(animal=c("dog", "cat", "sea cucumber", "sea urchin"),
                           feel=c("furry", "furry", "squishy", "spiny"),
                           weight=c(45, 8, 1.1, 0.8))
str(example_data)
```

```
## 'data.frame':	4 obs. of  3 variables:
##  $ animal: Factor w/ 4 levels "cat","dog","sea cucumber",..: 2 1 3 4
##  $ feel  : Factor w/ 3 levels "furry","spiny",..: 1 1 3 2
##  $ weight: num  45 8 1.1 0.8
```

```r
example_data <- data.frame(animal=c("dog", "cat", "sea cucumber", "sea urchin"),
                           feel=c("furry", "furry", "squishy", "spiny"),
                           weight=c(45, 8, 1.1, 0.8), stringsAsFactors=FALSE)
str(example_data)
```

```
## 'data.frame':	4 obs. of  3 variables:
##  $ animal: chr  "dog" "cat" "sea cucumber" "sea urchin"
##  $ feel  : chr  "furry" "furry" "squishy" "spiny"
##  $ weight: num  45 8 1.1 0.8
```

### Exercises

1. There are a few mistakes in this hand crafted `data.frame`, can you spot and
fix them? Don't hesitate to experiment!

   
   ```r
   ##  There are a few mistakes in this hand crafted `data.frame`,
   ##  can you spot and fix them? Don't hesitate to experiment!
   author_book <- data.frame(author_first=c("Charles", "Ernst", "Theodosius"),
                                author_last=c(Darwin, Mayr, Dobzhansky),
                                year=c(1942, 1970))
   ```

1. Can you predict the class for each of the columns in the following example?

   
   ```r
   ## Can you predict the class for each of the columns in the following example?
   ## Check your guesses using `str(country_climate)`. Are they what you expected?
   ##  Why? why not?
   country_climate <- data.frame(country=c("Canada", "Panama", "South Africa", "Australia"),
                                  climate=c("cold", "hot", "temperate", "hot/temperate"),
                                  temperature=c(10, 30, 18, "15"),
                                  north_hemisphere=c(TRUE, TRUE, FALSE, "FALSE"),
                                  has_kangaroo=c(FALSE, FALSE, FALSE, 1))
   ```

   Check your guesses using `str(country_climate)`. Are they what you expected?
   Why? why not?

   R coerces (when possible) to the data type that is the least common
   denominator and the easiest to coerce to.


## Inspecting `data.frame` objects

We already saw how the functions `head()` and `str()` can be useful to check the
content and the structure of a `data.frame`. Here is a non-exhaustive list of
functions to get a sense of the content/structure of the data.

* Size:
	* `dim()` - returns a vector with the number of rows in the first element, and
	  the number of columns as the second element (the __dim__ensions of the object)
	* `nrow()` - returns the number of rows
	* `ncol()` - returns the number of columns
* Content:
	* `head()` - shows the first 6 rows
	* `tail()` - shows the last 6 rows
* Names:
	* `names()` - returns the column names (synonym of `colnames()` for `data.frame`
	objects)
	* `rownames()` - returns the row names
* Summary:
	* `str()` - structure of the object and information about the class, length and
	content of  each column
	* `summary()` - summary statistics for each column

Note: most of these functions are "generic", they can be used on other types of
objects besides `data.frame`.


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



1. Use the function `subset` to create a `data.frame` that contains all
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



```r
download.file("http://r-bio.github.io/data/species.csv", "data/species.csv")
```


```r
species <- read.csv("data/species.csv", stringsAsFactors=FALSE)
```

We are then going to use the function `match()` to create a vector that contains
the genus names for all our observations. The function `match()` takes at least
2 arguments: the values to be matched (in our case the 2 letter code from the
`surveys` data frame held in the column `species`), and the table that contains
the values to be matched against (in our case the 2 letter code in the `species`
data frame held in the column `species_id`). The function returns the position
of the matches in the table, and this can be used to retrieve the genus names:


```r
surveys_spid_index <- match(surveys$species_id, species$species_id)
surveys_genera <- species$genus[surveys_spid_index]
```

Now that we have our vector of genus names, we can add it as a new column to our
`surveys` object:


```r
surveys <- cbind(surveys, genus=surveys_genera)
```

### Challenge

Use the same approach to also include the species names in the `surveys` data
frame.







```r
## and check out the result
head(surveys)
```

```
##   X record_id month day year plot species_id sex wgt       genus
## 1 1         1     7  16 1977    2         NL   M  NA     Neotoma
## 2 2         2     7  16 1977    3         NL   M  NA     Neotoma
## 3 3         3     7  16 1977    2         DM   F  NA   Dipodomys
## 4 4         4     7  16 1977    7         DM   M  NA   Dipodomys
## 5 5         5     7  16 1977    3         DM   M  NA   Dipodomys
## 6 6         6     7  16 1977    1         PF   M  NA Perognathus
##   species_name
## 1           NL
## 2           NL
## 3           DM
## 4           DM
## 5           DM
## 6           PF
```

* Use the help in R to understand what the function `paste()` does. Use it to
  add a new column called `genus_species` into the `species` `data.frame`.
* Use the help to understand what the function `merge()` does. Use it to create
  a new `data.frame` that combines the content of `surveys` and the modified
  version of `species`.
* Use this data set to answer the following:
  - How many birds have been captured?
  - How many individuals of the genus



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

* Using a similar approach, construct a new `data.frame` that only includes data
for the years 2000 and 2001.



* How does it differ from `subset(surveys, species == "DO" | species == "DM")


# Removing columns



Just like you can select columns by their positions in the `data.frame` or by
their names, you can remove them similarly.

To remove it by column number:


```r
surveys_noDate <- surveys[, -c(2:4)]
colnames(surveys)
```

```
##  [1] "X"            "record_id"    "month"        "day"         
##  [5] "year"         "plot"         "species_id"   "sex"         
##  [9] "wgt"          "genus"        "species_name"
```

```r
colnames(surveys_noDate)
```

```
## [1] "X"            "year"         "plot"         "species_id"  
## [5] "sex"          "wgt"          "genus"        "species_name"
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
## [1] "X"            "record_id"    "plot"         "species_id"  
## [5] "sex"          "wgt"          "genus"        "species_name"
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
unique(surveys$species_id)
```

```
##  [1] "NL" "DM" "PF" "PE" "DS" "PP" "SH" "OT" "DO" "OX" "SS" "OL" "RM" ""  
## [15] "SA" "PM" "AH" "DX" "AB" "CB" "CM" "CQ" "RF" "PC" "PG" "PH" "PU" "CV"
## [29] "UR" "UP" "ZL" "UL" "CS" "SC" "BA" "SF" "RO" "AS" "SO" "PI" "ST" "CU"
## [43] "SU" "RX" "PB" "PL" "PX" "CT" "US"
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
tapply(surveys_complete$wgt, surveys_complete$species_id, mean)
```

```
##         BA         DM         DO         DS         NL         OL 
##   8.600000  43.157864  48.870523 120.130546 159.245660  31.575258 
##         OT         OX         PB         PE         PF         PH 
##  24.230556  21.000000  31.735943  21.586508   7.923127  31.064516 
##         PI         PL         PM         PP         PX         RF 
##  19.250000  19.138889  21.364155  17.173942  19.000000  13.386667 
##         RM         RO         RX         SF         SH         SO 
##  10.585010  10.250000  15.500000  58.878049  73.148936  55.414634 
##         SS 
##  93.500000
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
