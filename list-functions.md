---
layout: page
title: List of functions you should be familiar with
---

This is inspired from/a subset of
[Hadley Wickham's vocabulary](http://adv-r.had.co.nz/Vocabulary.html).

```r
?   # to get help
str # structure of an object


$, [, [[ # to access elements from a list / data.frame

head, tail # begin and end of an object

## test content of vector
<, >, <=, >=
==, !=
duplicated, unique
order, rank, quantile
sort

## logical operations
!, &, !
all, any
which

## tabulate content of vector
table

## sets
intersect, union, setdiff, setequal

## Missing data and friends
is.na
is.null
is.nan
complete.cases

## Size of objects
length, dim, nrow, ncol

## Names
names
colnames, rownames
unname

## Add columns, rows
cbind, rbind
merge

## Basic statistics
min, max, range
mean, median, sd, cor, sd, var

prod, sum, cumsum, cumprod, diff

## control flow
if, else
for
ifelse
next, break
switch

## apply and friends
apply
tapply
lapply, sapply
replicate

## creating/manipulating vectors
c
rep, rep_len
seq, seq_len, seq_along
rev
sample

## coercion and tests
is/as.(character, numeric, logical, factor, ...)

## Character manipulation
grep, agrep
gsub
strsplit
chartr
nchar
tolower, toupper
substr
paste

## Factors
factor, levels, nlevels
reorder, relevel
cut, findInterval
interaction

## Output
print, cat
message, warning
dput
format
sink, capture.output

## Reading and writing data
data
count.fields
read.csv, write.csv
readLines, writeLines
readRDS, saveRDS
load, save

## Output
print, cat
message, warning, stop
dput
format
sink, capture.output

```
