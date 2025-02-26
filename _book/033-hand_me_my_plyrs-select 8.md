



# select

Written by Yena Joo.

## Introduction

### What is `select()`?

In this lesson, we will learn how to use the `select` function.  


When you're working with data, you find there are way too many variables in it, and some would wonder, how do I select only the variables I want to use for the analysis? Well, there is a super easy way to see the variables you choose by referring to variables based on the name of the column, with just one simple function.  

`select()` is a function that keeps only the variables you specify.  

The output of the function is a subset of the input data (columns), potentially with a different order. However, the function `select()` does not mutate the original dataset/columns. So if you want to use the new columns you selected, you will have to assign the value to a new variable.  

### Prerequisite skills include  
- You should have a good understanding of data (column, row, variable), and how to import data.  
- You also have to know how to use pipe (`%>%`) 

### Basic setup


```r
library(tidyverse)
```


## Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/4_8QnnKuO0M" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Basics

Here is a simple dataset that has average temperatures for each season. As you can see in the outcome, there are 3 observations(rows) and 4 variables(columns) in the dataset.


```
#> # A tibble: 3 × 4
#>   spring summer  fall winter
#>    <dbl>  <dbl> <dbl>  <dbl>
#> 1      3     23    19      2
#> 2      5     27    17     -1
#> 3     10     25    14     -8
```

Now, let's say we want to only see temperatures in the Spring. To do so, select variables you would like to keep by putting the variable name in the parameter with the function `select`.  
Here, I would like to just see the `spring` column by writing the code as below:


```r
select(temperature_data, spring)
#> # A tibble: 3 × 1
#>   spring
#>    <dbl>
#> 1      3
#> 2      5
#> 3     10
```

However, it is important to know that the data in `temperature_data` did not change as you can see. The function does not mutate the original dataset.


```r
temperature_data
#> # A tibble: 3 × 4
#>   spring summer  fall winter
#>    <dbl>  <dbl> <dbl>  <dbl>
#> 1      3     23    19      2
#> 2      5     27    17     -1
#> 3     10     25    14     -8
```


If you want to use the new dataset with the variable `spring`, you would have to assign the selected column(s) to a new variable:


```r
new_data = select(temperature_data, spring)
new_data
#> # A tibble: 3 × 1
#>   spring
#>    <dbl>
#> 1      3
#> 2      5
#> 3     10
```




## Operators 

Now, we know how to use the function `select` bare minimum. There are various ways to use the function with some operators.  


**1. You can select multiple variables using commas. Order of the input matters!**    


```r
select(temperature_data, winter, summer)
#> # A tibble: 3 × 2
#>   winter summer
#>    <dbl>  <dbl>
#> 1      2     23
#> 2     -1     27
#> 3     -8     25
```


**2. Another way of selecting multiple variables is to use the operator `c()`.**  
`c()` is a function that returns a vector(a one-dimensional array). Order of the input also matters.  


```r
temperature_data %>%
select(c(winter, fall))
#> # A tibble: 3 × 2
#>   winter  fall
#>    <dbl> <dbl>
#> 1      2    19
#> 2     -1    17
#> 3     -8    14
```
  
  

**3. The `!` operator negates a selection, `&` operator means "and"(intersection), whereas `|` takes the union of the selections (or).**


```r
temperature_data %>%
select(!winter)
#> # A tibble: 3 × 3
#>   spring summer  fall
#>    <dbl>  <dbl> <dbl>
#> 1      3     23    19
#> 2      5     27    17
#> 3     10     25    14
select(temperature_data, !winter & !summer)
#> # A tibble: 3 × 2
#>   spring  fall
#>    <dbl> <dbl>
#> 1      3    19
#> 2      5    17
#> 3     10    14
```

`select(temperature_data, winter & summer)` would incur an error because there cannot exist a variable that is both winter and summer.    



**4. The `:` operator selects a range of consecutive variables, starting from the variable you put on the left of the colon to the variable you put on the right of the colon.**  


```r
temperature_data %>% 
  select(summer:winter)
#> # A tibble: 3 × 3
#>   summer  fall winter
#>    <dbl> <dbl>  <dbl>
#> 1     23    19      2
#> 2     27    17     -1
#> 3     25    14     -8
```


**5. The `-` operator excludes a column.**  

If you would like to choose most of the columns in the dataset, and exclude a few columns, there is an easier way. You can just put `-` in front of the name of the column you would like to exclude. For example, I would like to exclude columns `summer` and `winter`, then I would just put a `-` in front of the columns:    


```r
temperature_data %>% 
  select(-summer, -winter)
#> # A tibble: 3 × 2
#>   spring  fall
#>    <dbl> <dbl>
#> 1      3    19
#> 2      5    17
#> 3     10    14
```

## Advanced uses

Some would wonder how they could select the columns based on the data types, since most of the statistical analyses use quantitative data rather than qualitative data. In that case, you can use the following:  

`select(which(sapply(., is.numeric)))`  

Instead of `is.numeric`, you can put `is.character` and `is.double` depending on what type of variables you would like to select in the dataset. 
Let's look at some examples. Here is a dataset that contains information about Sakura blooming in Japan, and it has various data types such as `<int>`(integer), `<chr>`(character), `<dbl>`(double). 


```r
# japanese_blooming <- read.csv("https://raw.githubusercontent.com/tacookson/data/master/sakura-flowering/temperatures-modern.csv")
# head(japanese_blooming)
```

Using the `which(sapply(., is.character))`, we can select the variables that have a data type character `<chr>`.  


```r
# japanese_blooming %>% select(which(sapply(.,is.character)))
```

If you want to select quantitative/numeric data, you can put `is.numeric` instead.  


```r
# japanese_blooming %>% select(which(sapply(.,is.numeric)))
```

  

Another way to perform, is the following using `select_if` with whichever data type you would like to select, such as `is.double`, `is.integer`, `is.double` and `is.character`:


```r
# japanese_blooming %>% select_if(is.double)
```

Selecting variables depending on the data type will come in handy when the dataset has hundreds of variables and you would like to select only quantitative variables for your data analysis/building a statistical model. 



## Exercises

Based on the material we have learned, now let's do some exercises.  


### Question 1

Modify this code so that we can only see from second to fourth column.


```r
temperature_data <- tibble(spring = c(3, 5, 10), 
                  summer = c(23, 27, 25), 
                  fall = c(19, 17, 14), 
                  winter = c(2, -1, -8)) 

select(temperature_data, 4:4)
#> # A tibble: 3 × 1
#>   winter
#>    <dbl>
#> 1      2
#> 2     -1
#> 3     -8
```

```r
select(temperature_data, 2:4)
#> # A tibble: 3 × 3
#>   summer  fall winter
#>    <dbl> <dbl>  <dbl>
#> 1     23    19      2
#> 2     27    17     -1
#> 3     25    14     -8
```

The outcome should be: 

```
#> # A tibble: 3 × 3
#>   summer  fall winter
#>    <dbl> <dbl>  <dbl>
#> 1     23    19      2
#> 2     27    17     -1
#> 3     25    14     -8
```
  

### Question 2 

Modify this code so that we can only see the columns "winter" and "summer" respectively, using `|` operator. 


```r
temperature_data <- tibble(spring = c(3, 5, 10), 
                  summer = c(23, 27, 25), 
                  fall = c(19, 17, 14), 
                  winter = c(2, -1, -8)) 

select(temperature_data, spring)
#> # A tibble: 3 × 1
#>   spring
#>    <dbl>
#> 1      3
#> 2      5
#> 3     10
```

```r
select(temperature_data, winter|summer)
#> # A tibble: 3 × 2
#>   winter summer
#>    <dbl>  <dbl>
#> 1      2     23
#> 2     -1     27
#> 3     -8     25
```

The correct answer should have a result like: 


```
#> # A tibble: 3 × 2
#>   winter spring
#>    <dbl>  <dbl>
#> 1      2      3
#> 2     -1      5
#> 3     -8     10
```



### Question 3

<!-- ```{r q3_select, echo=F} -->
<!-- question_checkbox( -->
<!--   "There are variables 'id', 'gpa', 'age', 'height', 'weight' are in a dataset 'data'. You would like to select only variables 'id', 'height', and 'weight'. What should I write? (Select all that apply) ", -->
<!--   answer("data %>% select(id, height, weight)", correct = T), -->
<!--   answer("data %>% select(id & height & weight)", message = "When is the `&` operator used? Check the 'Operations' section."), -->
<!--   answer("data %>% select(id | height | weight)", correct = T), -->
<!--   answer("data %>% select(id | height:weight)", correct = T), -->
<!--   allow_retry = T, -->
<!--   random_answer_order = T, -->
<!--   incorrect = "Try again. You got this!" -->
<!-- ) -->
<!-- ``` -->

### Question 4

<!-- Here is the dataset about ratings of children's books called `child_book_data`.  -->
<!-- ```{r echo = F} -->
<!-- #x <- getURL("https://raw.githubusercontent.com/tacookson/data/master/childrens-book-ratings/childrens-books-normalized-ratings.txt") -->
<!-- child_book_data <- read.delim("https://raw.githubusercontent.com/tacookson/data/master/childrens-book-ratings/childrens-books-normalized-ratings.txt") -->
<!-- head(child_book_data) -->
<!-- ``` -->

Select the columns that only are type `<dbl>` or if the column has a name `isbn`.

<!-- ```{r q4_select, exercise.eval = TRUE, exercise=TRUE} -->
<!-- child_book_data <- read.delim("https://raw.githubusercontent.com/tacookson/data/master/childrens-book-ratings/childrens-books-normalized-ratings.txt") -->
<!-- head(child_book_data) -->

<!-- child_book_data %>% select(isbn) -->
<!-- ``` -->

<!-- ```{r q4_select-solution} -->
<!-- child_book_data %>% select(isbn|which(sapply(.,is.double))) -->
<!-- ``` -->




## Common Mistakes & Errors

- If you don't have package "dplyr" or "tidyverse" installed and called in the library, the function would not work. Download either package using `install.packages("dplyr")`, and set it up in the library at the start of your code using `library(dplyr)`.  
- Make sure you typed in the correct variable/column name. Always check if your code contains any typo.  
- If you would like to use the new data frame using the variables you have selected, make sure to assign the selected variables to a new data frame.  
- Make sure you understand the differences between the operator `&` and `|`. They can be confusing.  


## Next Steps & See also 

In the tidyverse essentials, there are a ton of other functions you could mix & match with. After learning other functions such as `filter()`, `group_by()`, `arrange()`, `mutate()`, etc. you could easily modify the dataset according to your taste. 
  
## Summary

- package `tidyverse` or `dplyr` is needed.   
- `select()` keeps only the variables you mention.   
- There are some operators that can come in handy.  
  1. `|`: OR operator  
  2. `&`: AND operator  
  3. `c()`: a function that returns a vector, it is for choosing multiple columns. 
  4. `!`: to negate a statement or a column  
  5. `-`: to exclude a column   
  6. `:`: to select a range of consecutive variables  
- If you want to select columns with a specific data type, use `which(sapply(.,is.DATATYPE))` or use `select_if(is.DATATYPE)`  
  











