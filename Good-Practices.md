---
layout: page
title: Good practices for working with R
---

## Use functions

Whenever and wherever possible, encapsulate your scripts into functions. It
forces you to identify the steps involved in your analyses, it makes your code
cleaner, and will be easier to transpose these functions to other datasets. In
addition, having functions allows you to create documentation for them easily,
and will allow you to write tests to validate they behave as expected.

## Never save your workspace

It's never a good idea to start with a R session that is not clean. In RStudio,
change the default to never save your work space. Recreate the objects you need
from your script. If they take a long time to run (e.g., simulations), save
individual objects in a RDS file:

```{r}
saveRDS(some_big_object, file="data/rds/some_big_object.rds")
```

and load it as needed:

```{r}
some_big_object <- readRDS(file="data/rds/some_big_objects.rds")
```

## Never use `attach()`

The function `attach()` seems like a good idea as it could save you a lot of
typing. However, it will cause frustration and unexpected behaviors if you
forget to `detach()`. Play it safe, never use it.

## Never use `setwd()`

Except in a few edge cases, `setwd()` should only be typed directly in the
terminal by the user and should never be included in your scripts. Doing so will
make your scripts hard to use by other people as you may end up using paths that
are only on your computer.
