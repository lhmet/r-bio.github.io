---
layout: page
title: 08 -- String manipulations
---

We are going to use the package `stringr` to learn some common basic string
manipulation. In biology these are often needed to clean messy datasets, to work
with sequence data or extra data from unformatted data.

In base R, there are equivalent functions to the ones we will cover today, they
are a little more challenging to use but may provide additional flexibility if
you have very specific needs.

To learn the functions we'll cover today, we will work again with the list of
sea cucumber specimens downloaded from iDigBio.

Let's get set up


```r
## if you need to download the data
## download.file("http://r-bio.github.io/data/holothuriidae-specimens.csv",
##               "data/holothuriidae-specimens.csv")
hol <- read.csv(file="data/holothuriidae-specimens.csv", stringsAsFactors=FALSE)
library(stringr)
```

## Counting

The function `str_length()` gives the number of characters in a string:


```r
str_length(c("cat", "dog", "giraffe", "cute dog", "very cute kitten"))
```

```
## [1]  3  3  7  8 16
```

> ### Challenge
>
> Which country where sea cucumbers have been collected, has the most letters in
> its name?

## Extracting parts of a string based on number position

The function `str_sub()` can take 3 arguments: a vector of class "character", a
beginning and an end. Negative numbers indicates characters from the end (the
last one being -1).


```r
str_sub("a very cute kitten")
```

```
## [1] "a very cute kitten"
```

```r
str_sub("a very cute kitten", start=1L, end=-1L)
```

```
## [1] "a very cute kitten"
```

```r
str_sub("a very cute kitten", start=8)
```

```
## [1] "cute kitten"
```

```r
str_sub("a very cute kitten", start=-6)
```

```
## [1] "kitten"
```

```r
str_sub("a very cute kitten", start=3, end=6)
```

```
## [1] "very"
```

> ### Challenge
>
> A common mistake in taxonomic data is that the wrong suffixes are used in
>order or class names. Check that the last 4 letters are the same for all this
>dataset.

`str_sub()` can also be used to replace parts of a string:


```r
cutest <- "The cutest animals are puppies"
str_sub(cutest, -7) <- "kittens"
```


## Other useful functions

* `str_c()` equivalent to `paste()` but by default uses the empty strings as
separator:


```r
str_c("the cutest are ", c("cats", "dogs"), collapse=" and not ")
```

```
## [1] "the cutest are cats and not the cutest are dogs"
```

* `str_dup()` replicates a string as many times as specified


```r
str_dup(c("wow ", "amazing "), 3)
```

```
## [1] "wow wow wow "             "amazing amazing amazing "
```

* `str_trim()` removes leading and trailing spaces. It's very common when
  importing data (or cleaning up data entered in a form) that there are spaces
  that you don't want to have to deal with. This function removes
  them. `str_pad()` add whitespace (or other characters) on the right, left or
  both to make a string a given length (`width`)


```r
str_trim(str_dup(str_pad(c("wow ", "amazing "), width = str_length("amazing"), side="right", pad = "!"),  3))
```

```
## [1] "wow !!!wow !!!wow !!!"   "amazing amazing amazing"
```

## Pattern matching

> Some people, when confronted with a problem, think “I know, I'll use regular
> expressions.”  Now they have two problems.  Jamie Zawinski


Also see: [this](http://www.explainxkcd.com/wiki/index.php/1313:_Regex_Golf)



```r
sum(str_detect(hol$dwc.scientificName, ignore.case("holothuria")))
```

```
## Please use (fixed|coll|regexp)(x, ignore_case = TRUE) instead of ignore.case(x)
```

```
## [1] 2899
```


```r
library(dplyr)
authors <- hol$dwc.scientificNameAuthorship %>%
  str_replace(pattern = "å", replacement = "aa") %>%
  str_replace(pattern = "ä", replacement = "ae") %>%
  str_replace(pattern = "é", "e") %>%
  str_replace("^HL", "Hubert Lyman") %>%
  str_replace("^Krauss in", "") %>%
  str_split("&") %>% unlist() %>%
  str_split(", ") %>% unlist() %>%
    str_extract("[[:alpha:]]+(.+[[:alpha:]])?") %>%
    unique() %>% sort()
authors
```

```
##  [1] "Augustin"           "Bell"               "Brandt"            
##  [4] "Caso"               "Caycedo"            "Cherbonnier"       
##  [7] "Chiaje"             "Clark"              "Deichmann"         
## [10] "Delle Chiaje"       "Domantay"           "Erwe"              
## [13] "Feral"              "Fisher"             "Forskaal"          
## [16] "Gaimard"            "Gmelin"             "Hubert Lyman Clark"
## [19] "Jaeger"             "Kerr"               "Laguarda-Figueras" 
## [22] "Lampert"            "Lesson"             "Linnaeus"          
## [25] "Ludwig"             "Marenzeller"        "Massin"            
## [28] "Miller"             "Mitsukuri"          "Pawson"            
## [31] "Pourtales"          "Pourtalès"          "Purcell"           
## [34] "Quoy"               "Rowe"               "Samyn"             
## [37] "Selenka"            "Semper"             "Sluiter"           
## [40] "Solís-Marín"        "Tan Tiu"            "Theel"             
## [43] "Tomascik"           "Uthicke"            "von Marenzeller"
```

<!--- TODO: This lesson lacks a basic intro to regular expressions -->

## To learn more about string manipulations and regular expressions

* the package `stringr` has a good
  [vignette](https://github.com/hadley/stringr/blob/master/vignettes/stringr.Rmd)
  (on which this lecture is based)
* the package [`rex`](http://cran.r-project.org/web/packages/rex/index.html)
provides an easier way of writing regular expressions.
* This [website](http://www.regular-expressions.info/index.html) provides
reference and tutorials for regular expressions.
* This [tool](http://regexr.com/) allows you to check what your regular
expression will match on text you provide
