---
layout: page
title: 06 - More advanced functions, and introduction to Rmarkdown
---


# The problem

We want to answer a simple question: how many of the species of sea cucumbers
represented in iDigBio occur in the Red Sea?

>Note: You could actually answer this question in a couple of clicks with the
>interface of the iDigBio portal, but for the sake of this exercise we are going
>to pretend that it's not possible, and that we obtained the data from
>collaborators (who evidently haven't attended a data carpentry workshop yet.)

Currently, the data is in 178 files in CSV format. They represent all the
specimens of the sea cucumber genus _Holothuria_ that occur in iDigBio. However,
the data is split into multiple files (one file per species). Let's pretend that
you got these files from three different collaborators.

The file names are a little messy some are in the format of `H._species.csv`
others are in the `Hol_species.csv` format, and yet others are in the format
`holo_species.csv`. You forgot to agree with your collaborators on how to name
the files. Oops...

After inspecting the files, you also realize that you forgot to agree on which
order to record the information, and no two files have columns in the same
order. Oops... again.

Here is what we are going to do:

1. We are going to rename the files so they start with the family name, followed
   by the genus name spelled in full: _Holothuria_ It will make it easier to
   combine these files into a single spreadsheet.
1. We are going to create a single `data.frame` that combines the information
   for all species (but by only selecting some of the columns, currently each
   file as 200+ columns).
1. We are going to write a short paper about this project where we will use the
   skills we have learned to figure out how many of the species in this dataset
   occur in the Red Sea.
1. The paper will also include a map of the distribution of these species.

To do all this, we are going to write some functions and scripts to automate the
entire process process so we could regenerate the paper automatically if we
received more data or wanted to extend the study to other taxonomic groups.

Go to GitHub and fork [this repository](https://github.com/r-bio/intro-rmarkdown).

Create a new project in RStudio, choose "version control" and import your
fork. The address of the repository will be:
`https://github.com/YOUR_USERNAME/intro_rmarkdown`.

Go to the shell and add the upstream repo:

```
git remote add upstream https://github.com/r-bio/intro-rmarkdown
```

# Step 1: renaming the files

Renaming the files is a little tricky given what we've covered so far, so I'm
just going to give you the function to do it, and you can use it directly. Don't
hesitate to comment it while I explain what does what.

Open the `cleanup-function.R` file.


```r
rename_holothuria <- function(input_path="data_original/",
                              output_path="data_output/split_files",
                              pattern="(^H\\.|^Hol|^holo)",
                              family="Holothuriidae") {
    ## List all the files
    oldNm <- list.files(pattern=pattern, path=input_path)
    if (length(oldNm) < 1) {
        stop("No file found.")
    }

    ## Replace the abbreviated genus names with the full one
    fullGenus <- gsub(pattern, "Holothuria", oldNm)
    newNm <- paste(family, fullGenus, sep="-")

    ## Create a 2-column object that holds the old name and the common name
    nm <- cbind(file.path(input_path, oldNm), file.path(output_path, newNm))

    ## Copy the files to the data_output/split_files directory
    testRnm <- apply(nm, 1, function(x) file.copy(x[1], x[2]))

    if(all(testRnm)) {
        message("All good!")
        TRUE
    } else {
        warning("Something went wrong.")
        FALSE
    }
}
```

Use the function `rename_holothuria()` to copy and rename the files
correctly. They will be written in the folder `data_output/split_files`. This
function doesn't return anything useful so you don't need to assign a value to
it when running it.

If you see the message: `All good!`, put your green sticky up; if not, put your
red stick up.

# Step 2: collecting the information needed

Now that we have all files correctly named, we need to build a single
`data.frame` that only contains the columns we will need. I have started to
write a function to do this but you need to complete it to make it work. Replace
the `...` inside the function below.

>Note: if you are experienced with R, you may notice that the code here is not
>optimal. You could make it faster by instead of creating a 0-row data.frame,
>preallocating an empty data.frame with the correct dimensions (or an
>approximation of). Feel free to modify the code to do so. You could also
>vectorize the for loop.


```r
collect_holothuria <- function(pattern="^Holothuriidae-", path="data_output/split_files") {

    holFiles <- list.files(path=path, pattern=pattern)

    ## create empty data.frame to store the results
    res <- data.frame()

    for (i in 1:length(holFiles)) {
        tmpFile <- read.csv(file.path(path, holFiles[i]), stringsAsFactors=FALSE)

        ## write a command that selects the following columns from tmpFile.
        ## dwc.family
        ## dwc.genus
        ## dwc.specificEpithet
        ## idigbio.uuid
        ## dwc.scientificName
        ## dwc.occurrenceID
        ## dwc.decimalLatitude
        ## dwc.decimalLongitude
        tmpFile <- ...

        ## write a command that adds these data to our storage data.frame `res`
        res <- ...
    }
    res
}
```

<!---

--->

# Step 3: saving the output

Use the function `write.csv()` to create a CSV file of this newly generated and
clean `data.frame`. Name your file `occurrence_holothuria.csv` and save it
inside your `data_output` folder.

We are now ready to write our paper about the sea cucumber fauna of the Red Sea.

Make sure that you have the following packages installed:

1. ggplot2
1. maps
1. knitr

and open the file `paper.Rmd`
