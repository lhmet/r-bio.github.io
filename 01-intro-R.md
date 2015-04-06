---
layout: page
title: 01 -- First lecture
time: 2.5 hours
---

# Preamble

This first lecture is divided in 3 parts.

First, we will review the motivations of why learning R?

Second, we will go through a first session with R to get used to the syntax and
learn how to generate a simple plot.

Third, we will explore vectors as they are at the heart on how objects are
stored in R's memory.

------

# Motivation - Why learn R?

## R is not a GUI, and that's a good thing

The learning curve might be steeper than with other software, but with R, you
can save all the steps you used to go from the data to the results. So, if you
want to redo your analysis because you collected more data, you don't have to
remember which button you clicked in which order to obtain your results, you
just have to run your script again.

Working with scripts makes the steps you used in your analysis clear, and the
code you write can be inspected by someone else who can give you feedback and
spot mistakes.

Working with scripts forces you to have deeper understanding of what you are
doing, and facilitates your learning and comprehension of the methods you use.


## R code is great for reproducibility

Reproducibility is when someone else (including your future self) can obtain the
same results from the same dataset when using the same analysis.

R integrates with other tools to generate manuscripts from your code. If you
collect more data, or fix a mistake in your dataset, the figures and the
statistical tests in your manuscript are updated automatically.

An increasing number of journals and funding agencies expect analyses to be
reproducible, knowing R will give you an edge with these requirements.


## R is interdisciplinary and extensible

With 6,000+ packages that can be installed to extend its capabilities, R
provides a framework that allows you to combine analyses across many scientific
disciplines to best suit the analyses you want to use on your data. For
instance, R has packages for image analysis, GIS, time series, population
genetics, and a lot more.


## R works on data of all shapes and size

The skills you learn with R scale easily with the size of your dataset. Whether
your dataset has hundreds or millions of lines, it won't make much difference to
you.

R is designed for data analysis. It comes with special data structures and data
types that make handling of missing data and statistical factors convenient.

R can connect to spreadsheets, databases, and many other data formats, on your
computer or on the web.


## R produces high-quality graphics

The plotting functionalities in R are endless, and allow you to adjust any
aspect of your graph to convey most effectively the message from your data.


## R has a large community

Thousands of people use R daily. Many of them are willing to help you through
mailing lists and stack overflow.


## Not only R is free, but it is also open-source and cross-platform

Anyone can inspect the source code to see how R works. Because of this
transparency, there is less chance for mistakes, and if you (or someone else)
find some, you can report and fix bugs.

-----

# A first R session

* Start RStudio
* Under the `File` menu, click on `New project`, choose `New directory`, then
  `Empty project`
* Enter a name for this new folder, and choose a convenient location for
  it. This will be your **working directory** for the rest of the day
  (e.g., `~/R-class/week1`)
* Click on "Create project"
* Under the `Files` tab on the right of the screen, click on `New Folder` and
  create a folder named `data` within your newly created working directory.
  (e.g., `~/R-class/week1/data`)
* Create a new R script (File > New File > R script) and save it in your working
  directory (e.g. `my-first-script.R`)

## Good practices

There are two main ways of interacting with R: using the console or by using
script files (plain text files that contain your code).

The recommended approach when working on a data analysis project is dubbed "the
source code is real". The objects you are creating should be seen as disposable
as they are the direct realization of your code. Every object in your analysis
can be recreated from your code, and all steps are documented. Therefore, it is
best to enter as little commands as possible in the R console. Instead, all code
should be written in script files, and evaluated from there. The R console
should be used to inspect objects, test a function or get help. With this
approach, the `.Rhistory` file automatically created during your session should
not be very useful.

Similarly, you should separate the original data (raw data) from intermediate
datasets that you may create for the need of a particular analysis. For
instance, you may want to create a `data/` directory within your working
directory that stores the raw data, and have a `data_output/` directory for
intermediate datasets and a `figure_output/` directory for the plots you will
generate.

## Creating objects

Let's start by creating a simple object:

```r
x <- 10
x
```

We assigned to `x` the number 10. `<-` is the assignment operator. Assigns
values on the right to objects on the left. Mostly similar to `=` but not
always. Learn to use `<-` as it is good programming practice. Using `=` in place
of `<-` can lead to issues down the line.

`=` should only be used to specify the values of arguments in functions for
instance `read.csv(file="data/some_data.csv")`.

We can now manipulate this value to do things with it. For instance:

```r
x * 2
x + 5
x + x
```

or we can create new objects using `x`:

```r
y <- x + x + 5
```

Let's try something different:

```r
x <- c(2, 4, 6)
x
```

Two things:

- we overwrote the content of `x`
- `x` now contains 3 elements

Using the `[]`, we can access individual elements of this object:

```r
x[1]
x[2]
x[3]
```

---

### Challenge

What is the content of this vector?

```r
q <- c(x, x, 5)
```

---

We can also use these objects with functions, for instance to compute the mean
and the standard deviation:

```r
mean(x)
sd(d)
```

This is useful to print the value of the mean or the standard deviation, but we
can also save these values in their own variables:

```r
mean_x <- mean(x)
mean_x
```

The function `ls()` returns the objects that are currently in the memory of
your session.

The function `data()` allows you to load into memory datasets that are provided
as examples with R (or some packages). Let's load the `Nile` dataset that
provides the annual flow of the river Nile between 1871 and 1970.

```r
data(Nile)
```

Using `ls()` shows you that the function `data()` made the variable `Nile`
available to you.

Let's make an histogram of the values of the flows:

```r
hist(Nile)
```

---

### Challenge

The following: `abline(v=100, col="red")` would draw a vertical line on an
existing plot at the value 100 colored in red.

How would you add such a line to our histogram to show where the mean falls in
this distribution?

---

We can now save this plot in its own file:

```r
pdf(file="nile_flow.pdf")
hist(Nile)
abline(v=mean(Nile), col="red")
dev.off()
```

------

# Vectors

Vectors are at the heart of how data are stored into R's memory. Almost
everything in R is stored as a vector. When we typed `x <- 10` we created a
vector of length 1. When we typed `x <- c(2, 4, 6)` we created a vector of
length 3. These vectors are of class `numeric`. Vectors can be of 6 different
classes (we'll mostly work with 4).

### The different "classes" of vector

* `"numeric"` is the general class for vectors that hold numbers (e.g., `c(1, 5,
  10)`)
* `"integer"` is the class for vectors for integers. To differentiate them from
  `numeric` we must add an `L` afterwards (e.g., `c(1L, 2L, 5L)`)
* `"character"` is the general class for vectors that hold text strings (e.g.,
  `c("blue", "red", "black")`)
* `"logical"` for holding `TRUE` and `FALSE` (boolean data type)

The other types of vectors are `"complex"` (for complex numbers) and `"raw"` a
special internal type that is not of use for the majority of users.

### How to create vectors?

The easiest way is to create them directly as we have done before:

```r
x <- c(5, 10, 15, 20, 25)
class(x)
```

However, there will be cases when we want to create empty vectors that will be
later populated with values.

```r
x <- numeric(5)
x
```

Similarly, we can create empty vectors of class `"character"` using
`character(5)`, or of class `"logical"`: `logical(5)`, etc.

### Naming the elements of a vector

```r
fav_colors <- c("red", "blue", "green", "yellow")
names(fav_colors)
names(fav_colors) <- c("John", "Lucy", "Greg", "Sarah")
fav_colors
names(fav_colors)
unname(fav_colors)
```

### How to access elements of a vector?

They can be accessed by their indices:

```r
fav_colors[2]
fav_colors[2:4]
```

repeatitions are allowed:

```r
fav_colors[c(2,3,2,4,1,2)]
```

or if the vector is named, it can be accessed by the names of the elements:

```r
fav_colors["John"]
```

---

### Challenges

* How to access the content of the vector for "Lucy", "Sarah" and "John" (in this
order)?
* How to get the name of the second person?

---

### How to update/replace the value of a vector?

```r
x[4] <- 22
```

```r
fav_colors["Sarah"] <- "turquoise"
```


### How to add elements to a vector?

```r
x <- c(5, 10, 15, 20)
x <- c(x, 25) # adding at the end
x <- c(0, x)  # adding at the beginning
x
```

With named vectors:

```r
fav_colors
c(fav_colors, "purple")
fav_colors <- c(fav_colors, "Tracy" = "purple")
```

Notes:

* here is the case where using the `=` is OK/needed
* pay attention to where the quotes are

---

### Challenge

* If we add another element to our vector:

```r
fav_color <- c(fav_colors, "black")
```

how to use the function `names()` to assign the name "Ana" to this last element?

---


### How to remove elements from a vector?

```r
x[-5]
x[-c(1, 3, 5)]
```

but this `fav_colors[-c("Tracy")]` does not work. We need to use the function
`match()`:

```r
fav_colors[-match("Tracy", names(fav_colors))]
```

The function `match()` looks for the position of the **first exact match**
within another vector.


### Sequences

`:` is a special function that creates numeric vectors of integer in increasing
or decreasing order, test `1:10` and `10:1` for instance. The function `seq()`
(for __seq__uence) can be used to create more complex patterns:


```r
seq(1, 10, by=2)
```

```
## [1] 1 3 5 7 9
```

```r
seq(5, 10, length.out=3)
```

```
## [1]  5.0  7.5 10.0
```

```r
seq(50, by=5, length.out=10)
```

```
##  [1] 50 55 60 65 70 75 80 85 90 95
```

```r
seq(1, 8, by=3) # sequence stops to stay below upper limit
```

```
## [1] 1 4 7
```

```r
seq(1.1, 2, length.out=10)
```

```
##  [1] 1.1 1.2 1.3 1.4 1.5 1.6 1.7 1.8 1.9 2.0
```

### Repeating

```r
x <- rep(8, 4)
x
rep(1:3, 3)
```

### Operations on vectors

```r
x <- c(5, 10, 15)
x + 10
x + c(10, 15, 20)
x * 10
x * c(2, 4, 3)
```

Note that operations on vectors are elementwise.

### Recycling

R allows you to do operations on vectors of different lengths. The shorter
vector will be "recycled" (~ repeated) to match the length of the longer one:

```r
x <- c(5, 10, 15)
x + c(2, 4, 6, 8, 10, 12)     # no warning when it's a multiple
x + c(2, 4, 6, 8, 10, 12, 14) # warning
```

### Boolean operations and Filtering

```r
u <- c(1, 4, 2, 5, 6, 3, 7)
u < 3
u[u < 3]
u[u < 3 | u >= 4]
u[u > 5 & u < 1 ] ## nothing matches this condition
u[u > 5 & u < 8]
```

With character strings:

```r
fav_colors <- c("John" = "red", "Lucy" = "blue", "Greg" = "green",
                "Sarah" = "yellow", "Tracy" = "purple")
fav_colors == "blue"
fav_colors[fav_colors == "blue"]
which(fav_colors == "blue")
names(fav_colors)[which(fav_colors == "blue")]
```
