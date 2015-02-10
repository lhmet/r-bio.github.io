---
layout: page
title: Challenges
---

## The sea cucumber challenge

For this challenge, you are going to use what you have learned in the first few
weeks to solve common issues when dealing with data, and perform a quick
exploration of the dataset. I tried to build the questions so that if you get
stuck, you can move on to the following questions.

I am providing you with two datasets:

- `holothuriidae-specimens.csv` contains a list of sea cucumber specimens housed
  at natural history museums across the United States. It's a simplification of
  a dataset I obtained through the
  [iDigBio portal](https://www.idigbio.org/portal).
- `holothuriidae-nomina-valid.csv` contains the list of currently accepted
  taxonomic names for sea cucumbers.

This is real raw data with errors and inconsistencies. The first dataset is
quite messy as it gathers information across several institutions, and for some
of them, the specimens haven't been examined in many years and may be identified
with a taxonomic name that is not currently valid.

Before we get started, let's get organized:

1. Start RStudio, and create a new project (File > New Project)
1. Choose "New Directory" and "Empty Project"
1. Call your Directory "cuke-challenge" and place it wherever is convenient for
   you on your hardrive.
1. Check the box "Create a git repository" before clicking the button "Create
   Project".
1. Create a new folder inside your working directory called `data`. You can do
   this using the "New Folder" icon in the File panel in RStudio or by typing
   `dir.create("data")` at the R console.
1. Create a new script file (File > New File > R script) and save it as
   `cuke-challenge.R`

Now, download the two data files inside your newly created `data` folder by
typing in your script file:


```r
download.file("http://r-bio.github.io/data/holothuriidae-specimens.csv",
              "data/holothuriidae-specimens.csv")
download.file("http://r-bio.github.io/data/holothuriidae-nomina-valid.csv",
              "data/holothuriidae-nomina-valid.csv")
```

and use the function `read.csv()` to load these datasets in memory. We'll call
`hol` the data frame that contains the information about the specimens, and
`nom` the data frame that contains the information about the validity of the
species names.


```r
hol <- read.csv(file="data/holothuriidae-specimens.csv", stringsAsFactors=FALSE)
nom <- read.csv(file="data/holothuriidae-nomina-valid.csv", stringsAsFactors=FALSE)
```

1. How many specimens are included in the data frame `hol`?
1. The column `dwc.institutionCode` in the `hol` data frame lists the museum
   where the specimens are housed:
   - How many institutions house specimens?
   - Draw a bar plot that shows the contribution of each institution
1. When was the oldest specimen included in this data frame collected? (hint: It
   was not in year 1)
1. What proportion of the specimens in this data frame were collected between
   the years 2006 and 2014 (included)?
1. The function `nzchar()` on a vector returns `TRUE` for the positions of the
   vectors that are not empty, and `FALSE` otherwise. For instance,
   `nzchar(c("a", "b", "", "", "e"))` would return the vector `c(TRUE, TRUE,
   FALSE, FALSE, TRUE). The column `dwc.class` is supposed to contain the Class
   information for the specimens (here they should all be
   "Holothuroidea"). However, it is missing for some. Use the function `nzchar`
   to answer:
   - How many specimens do not have the information for class listed?
   - For the specimens where the information is missing, replace it with the
     information for their (again, they should all be "Holothuroidea").
1. Using the `nom` data frame, and the column `Subgenus.current`, which
   of the genera listed has/have subgenera?
1. We want to combine the information included in the `nom` and the `hol`
  spreadsheets, to identify the specimens in the data frame that use species
  names that are not valid. We'll do this using the function `merge()`. By
  default `merge()` only returns the rows for which there is an exact match in
  both datasets. Here, because `nom` only includes the names of the valid
  species, the results would not include any of the specimen information that do
  not have valid names. Read the help of the `merge()` function to learn more
  about it.
  - With the function `paste()`, create a new column (called `genus_species`)
    that contains the genus (column `dwc.genus`) and species names (column
    `dwc.specificEpithet`) for the `hol` data frame.
  - Do the same thing with the `nom` data frame (using the columns
    `Genus.current` and `species.current`).
  - Use `merge()` to combine `hol` and `nom` (hint: you will need to use the
    `all.x` argument, read the help to figure it out, and check that the
    resulting data frame has the same number of rows as `hol`).
  - How many specimens are identified with a currently invalid species name?
    (hint: specimens identified only with a genus name shouldn't be included in
    this count?)
