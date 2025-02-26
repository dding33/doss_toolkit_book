



# `tidyr` package

*Written by Mariam Walaa.*


## Introduction

In this lesson, you will learn how to:

- Use additional `tidyr` functions, such as `unnest_wider()` and `unnest_longer()`

Prerequisite skills include:

- Previous tutorials involving `tidyr` functions

Highlights:

- We can use `tidyr` functions to put a non-tidy dataset into tidy format.
- unnest_wider() and unnest_longer() give flexibility in terms of how to unnest data.

## Overview

A common theme across working with datasets is standardizing the dataset format. Datasets
must be standardized. If every dataset was unique in its format, it would be difficult for
data scientists and data analysts to work on them. Everyone would have a vastly
different workflow needed to reach the analysis step, and people would have a harder time
collaborating with each other and evaluating each other's results. That is why datasets
must be *standardized*.

You may wonder how we can standardize a dataset when we have one, and to what extent we 
can standardize it. In [Hadley Wickham's Tidy Data
paper](https://vita.had.co.nz/papers/tidy-data.html) from 2014, Hadley introduces 3 rules
every dataset must follow in order to be considered tidy: Every row must be an observation,
every column must be a variable, and every cell is a single measurement. Here is an 
illustration that summarizes these 3 rules, by Allison Horst.

<img src="images/59_tidy-data.jpg" width="90%" />

Credits: Allison Horst

As the title says, this section will provide you with a summary of some functions you have
seen in previous tutorials, as well as introduce you to more functions from tidyr that
you have not seen yet.

<img src="images/59_tidy-data-2.jpg" width="90%" />

Credits: Allison Horst

## Example 

Suppose you are given this data that is not in tidy format. 




```r
nontidy_data
#> # A tibble: 3 × 4
#>   variable  `1`       `2`       `3`      
#>   <chr>     <list>    <list>    <list>   
#> 1 n_lines   <dbl [1]> <dbl [1]> <dbl [1]>
#> 2 n_figures <dbl [1]> <dbl [1]> <dbl [1]>
#> 3 n_scripts <dbl [1]> <dbl [1]> <dbl [1]>
```

First, the columns and rows are switched, and second, the cells are all hidden.

Here is the code we need to tidy it:


```r
nontidy_data %>%
  pivot_longer(cols = -variable, names_to = "name", values_to = "value") %>%
  pivot_wider(names_from = "variable") %>%
  unnest(everything())
#> # A tibble: 3 × 4
#>   name  n_lines n_figures n_scripts
#>   <chr>   <dbl>     <dbl>     <dbl>
#> 1 1         100         4        10
#> 2 2         200         5        20
#> 3 3         300         6        30
```

Lets go through this step by step and check the output each time.

To clean it, we will use our functions pivot_longer(), pivot_wider(), and unnest() from
tidyr.


```r
# 1. Convert to long format
nontidy_data_l <- nontidy_data %>%
  pivot_longer(cols = -variable, names_to = 'name', values_to = 'value')
nontidy_data_l
#> # A tibble: 9 × 3
#>   variable  name  value    
#>   <chr>     <chr> <list>   
#> 1 n_lines   1     <dbl [1]>
#> 2 n_lines   2     <dbl [1]>
#> 3 n_lines   3     <dbl [1]>
#> 4 n_figures 1     <dbl [1]>
#> 5 n_figures 2     <dbl [1]>
#> 6 n_figures 3     <dbl [1]>
#> 7 n_scripts 1     <dbl [1]>
#> 8 n_scripts 2     <dbl [1]>
#> 9 n_scripts 3     <dbl [1]>
```

Our dataset is in a long format now.


```r
# 2. Convert to wide format
nontidy_data_w <- nontidy_data_l %>%
  pivot_wider(names_from = 'variable')
nontidy_data_w
#> # A tibble: 3 × 4
#>   name  n_lines   n_figures n_scripts
#>   <chr> <list>    <list>    <list>   
#> 1 1     <dbl [1]> <dbl [1]> <dbl [1]>
#> 2 2     <dbl [1]> <dbl [1]> <dbl [1]>
#> 3 3     <dbl [1]> <dbl [1]> <dbl [1]>
```

Notice how step 2 brings the variable names to the top.


```r
# 3. Unnest (or unfold) the cells
tidy_data <- nontidy_data_w %>%
  unnest(everything())
tidy_data
#> # A tibble: 3 × 4
#>   name  n_lines n_figures n_scripts
#>   <chr>   <dbl>     <dbl>     <dbl>
#> 1 1         100         4        10
#> 2 2         200         5        20
#> 3 3         300         6        30
```

Now it is tidy data. You can also clean it up as follows:


```r
tidy_data %>%
  column_to_rownames('name')
#>   n_lines n_figures n_scripts
#> 1     100         4        10
#> 2     200         5        20
#> 3     300         6        30
```

## Exercises



We will be looking at a data set of Broadway shows with variables about the performances,
attendance, and revenue for theaters that are part of The Broadway League. You can learn
more about the data set provided by Alex Cookson in this [Git
repository](https://github.com/tacookson/data) as well as this corresponding [blog
post](https://www.alexcookson.com/post/most-successful-broadway-show-of-all-time/). Take a
look at a subset of this data for the Winter Garden Theatre.


```r
# winter_garden
```

You can Click Next to look through the observations.

### Exercise 1

<!-- ```{r tidyr-exercise-1, echo = FALSE} -->
<!-- question("Without using the nest() function, how many rows and columns do we have after nesting?", -->
<!--          answer("2 rows and 2 columns", correct = TRUE), -->
<!--          answer("44 rows and 2 columns"), -->
<!--          answer("44 rows and 3 columns"), -->
<!--          answer("2 rows and 3 columns"), -->
<!--          allow_retry = TRUE, -->
<!--          random_answer_order = TRUE) -->
<!-- ``` -->

<!-- #### Exercise 2 -->

<!-- Try the nest() function. Do you get the expected result? -->

<!-- ```{r tidyr-exercise-2, exercise = TRUE} -->

<!-- ``` -->

<!-- ```{r tidyr-exercise-2-sol, echo = FALSE} -->
<!-- tidyr_ex2_sol <- winter_garden %>% nest(data = c(show)) -->
<!-- ``` -->


<!-- #### Exercise 3 -->

<!-- ```{r tidyr-exercise-3, echo = FALSE} -->
<!-- question("Consider the code from Exercise 2. What would the sizes of each of the nested data frames be?", -->
<!--          answer("22 rows by 1 column", correct = TRUE), -->
<!--          answer("1 row by 22 columns"), -->
<!--          answer("44 rows by 1 column"), -->
<!--          answer("1 row by 44 columns"), -->
<!--          allow_retry = TRUE, -->
<!--          random_answer_order = TRUE) -->
<!-- ``` -->

<!-- #### Exercise 4 -->

<!-- ```{r tidyr-exercise-4, echo = FALSE} -->
<!-- question("Consider the code from Exercise 2. Without using the unnest_longer() function, how many rows and columns are in this dataset after unnesting longer?", -->
<!--          answer("22 rows by 1 column"), -->
<!--          answer("1 row by 22 columns"), -->
<!--          answer("44 rows by 2 columns", correct = TRUE), -->
<!--          answer("1 row by 44 columns"), -->
<!--          allow_retry = TRUE, -->
<!--          random_answer_order = TRUE) -->
<!-- ``` -->

<!-- #### Exercise 5 -->

<!-- Take a look at another subset of the data set, for Palace Theatre this time.  -->

<!-- ```{r tidyr-data-2-1} -->
<!-- palace -->
<!-- ``` -->

<!-- ```{r tidyr-exercise-5, echo = FALSE} -->
<!-- question("Which of these functions gives us 12 rows and 2 columns?", -->
<!--          answer("nest()", correct = TRUE), -->
<!--          answer("unnest()"), -->
<!--          answer("unnest_wider()"), -->
<!--          answer("unnest_longer()"), -->
<!--          allow_retry = TRUE, -->
<!--          random_answer_order = TRUE) -->
<!-- ``` -->

## Next Steps

- Try looking at `vignette("rectangle")`! This is more advanced than what you have seen in
this tutorial, but if you are interested, then this might be helpful.







