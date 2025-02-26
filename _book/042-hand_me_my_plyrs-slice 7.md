



# slice

Written by Annie Collins.


## Introduction

In this lesson, you will learn how to:

- Select rows in a data frame using the `slice()` function

This lesson is a yellow level skill and is part of "Tidyverse Essentials". Prerequisite skills include:

- Installing packages
- Calling libraries
- Importing data

## slice()

The `slice()` function is part of the R base package (the functions that come with R) with several additional variations in the `dplyr` package.

`slice()` allows you to select rows from your data by their location in the data frame. This can be done by inputting specific row numbers, ranges of row numbers, or by choosing rows to omit from the data. The `slice()` function does not manipulate the original data frame, but rather outputs a *copy* of the original data frame including only the selected rows.

The syntax for using slice is as follows:

\center `slice(data, row number(s) of row(s) to be kept/removed)` \center

This will be further explained in the following sections.

## Video Overview

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ob3hZJ0EUXM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Selecting Rows

We will be slicing the data frame below (named `pizza`) which includes data on different pizza types and their toppings. For the purposes of visualizing the `slice()` function, the row numbers have been indicated next to each pizza type.

```
#>              type      top1        top2
#> 1     (1) classic    cheese   pepperoni
#> 2    (2) hawaiian       ham   pineapple
#> 3      (3) veggie mushrooms     peppers
#> 4 (4) meat lovers   sausage       bacon
#> 5       (5) greek    olives feta cheese
```

### Single Row

If you wish to select a single row from your data frame, simply input the row's number into the `slice()` function following the name of your data.

The code below returns the information about a veggie pizza.

```r
slice(pizza, 3)
#>         type      top1    top2
#> 1 (3) veggie mushrooms peppers
```

### Multiple Rows
There are several ways to slice multiple rows at once.

You may input several integers separated by commas, similar to the above example of selecting a single row. The code below returns information about classic, veggie, and greek pizzas.

```r
slice(pizza, 1, 3, 5)
#>          type      top1        top2
#> 1 (1) classic    cheese   pepperoni
#> 2  (3) veggie mushrooms     peppers
#> 3   (5) greek    olives feta cheese
```

You can also input a vector of integers indicating the rows you wish to slice. This functions essentially the same as the previous method, but may be useful if you already have a numeric vector containing this information. The code below also returns information about classic, veggie, and greek pizzas.

```r
nums <- c(1, 3, 5)
slice(pizza, nums)
#>          type      top1        top2
#> 1 (1) classic    cheese   pepperoni
#> 2  (3) veggie mushrooms     peppers
#> 3   (5) greek    olives feta cheese
```

If you wish to select information from multiple adjacent rows, you can input a numeric range instead of selecting rows individually. The syntax for this is "first row:last row". The code below outputs the first three rows of `pizza`.

```r
slice(pizza, 1:3)
#>           type      top1      top2
#> 1  (1) classic    cheese pepperoni
#> 2 (2) hawaiian       ham pineapple
#> 3   (3) veggie mushrooms   peppers
```

### Omitting Rows
Another way of slicing rows is choosing which rows to *omit* or *remove* from the data frame. You can use any of the above methods to remove individual or multiple rows from a data frame by placing "**-**" before the inputted row number(s). The `slice()` function will then return all the rows in the data frame *except* the rows indicated. Uncomment each line of code below to observe the output.



```r
# Return everything except row 3
slice(pizza, -3)
#>              type    top1        top2
#> 1     (1) classic  cheese   pepperoni
#> 2    (2) hawaiian     ham   pineapple
#> 3 (4) meat lovers sausage       bacon
#> 4       (5) greek  olives feta cheese
# Return only rows 2 and 4
# slice(pizza, -1, -3, -5)
# OR
# nums <- c(1, 3, 5)
# slice(pizza, -nums)
# Return only the last two rows (4:5)
# slice(pizza, -1:-3)
```
Note that when using `slice()`, positive and negative numbers cannot be combined. All row number values must be either positive or negative, including in vectors and ranges.

It is also possible to combine selection methods, for instance by indicating a range of rows followed by another individual row (`slice(pizza, 1:3, 5)` will return everything except row 4).

## Questions & Exercises

Please use the dataset `olympics`, representing medal counts from the 2016 summer Olympics in Rio de Janeiro, for the following questions and exercises.

```
#>         country gold silver bronze
#> 1 United States   46     37     38
#> 2 Great Britain   27     23     17
#> 3         China   26     18     26
#> 4        Russia   19     17     20
#> 5       Germany   17     10     15
#> 6         Japan   12      8     21
#> 7        France   10     18     14
#> 8   South Korea    9      3      9
```

### Questions

<!-- ```{r slice-q1, echo=FALSE} -->
<!-- question("Which of the following is **not** equivalent to `slice(olympics, 1:2)?`", -->
<!-- answer("slice(olympics, 1, 2)"), -->
<!-- answer("slice(olympics, c(1, 2))"), -->
<!-- answer("slice(olympics, -3:-8)"), -->
<!-- answer("slice(olympics, 3:8)", correct=TRUE) -->
<!-- ) -->
<!-- ``` -->
<!-- ```{r slice-q2, echo=FALSE} -->
<!-- question("Which of the following will return data for all countries in `olympics`?", -->
<!-- answer("slice(olympics, 1:8)", correct=TRUE), -->
<!-- answer("slice(olympics, 8)"), -->
<!-- answer("slice(olympics, -1)"), -->
<!-- answer("slice(olympics, c(1, 8))") -->
<!-- ) -->
<!-- ``` -->

### Exercises



<!-- 1. Using `slice()` and a numeric vector, extract information for Russia, Germany, and Japan from `olympics`.  -->
<!-- ```{r sliceexercise1, exercise=TRUE} -->

<!-- ``` -->

<!-- ```{r sliceexercise1-solution} -->
<!-- vector <- c(4, 5, 6) -->
<!-- slice(olympics, vector) -->
<!-- ``` -->

<!-- 2. Using `slice()`, show information for all countries in `olympics` except for Great Britain and France. -->
<!-- ```{r sliceexercise2, exercise=TRUE} -->

<!-- ``` -->

<!-- ```{r sliceexercise2-solution} -->
<!-- slice(olympics, -2, -7) -->
<!-- ``` -->

<!-- 3. Using `slice()`, show only the top three gold medal winners from the 2016 Olympic games. -->
<!-- ```{r sliceexercise3, exercise=TRUE} -->

<!-- ``` -->

<!-- ```{r sliceexercise3-solution} -->
<!-- slice(olympics, 1:3) -->
<!-- ``` -->


## Common Mistakes

- **Slicing rows that do not exist**: Ensure you are always inputting row numbers that exist in your data. If you input row numbers that do not exist in your data (for example, `slice(pizza, 6)` when `pizza` only has 5 rows), the function will return *<0 rows> (or 0-length row.names)* which is an empty data frame. To check the number of rows in your data, you can use the function `nrow()`. You can also use `n()` to represent the number of the last row of your data regardless of length (ie. 1:n() would slice every row of a data frame).
- **Combining Positive and Negative Indexes**: As mentioned previously, all row number values must be either positive or negative when using `slice()`, including in vectors and ranges. If you combine positive and negative values, you may get the following message: *Error: \`slice()\` expressions should return either all positive or all negative.*.
- **"Losing" your sliced data frame**: When you use the `slice()` function, you are not directly changing your original data. If you want your data frame to be saved in its "sliced" form, you must reassign the name of your data frame to the output of the `slice()` function. For example, if I wanted to permanently remove the last two rows of `pizza`, I would execute the code `pizza <- slice(pizza, -4:-5)`.


## Next Steps

There are several functions that act as variations of `slice()` with similar syntax in the `dplyr` package. These include:

- `slice_head()` and `slice_tail()`, to select a number of first or last rows
- `slice_sample()`, to randomly select rows from a data frame
- `slice_min()` and `slice_max()`, to select rows with the highest or lowest value(s) of a specified variable.














