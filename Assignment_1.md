Assignment 1
================

``` r
library(datateachr)
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(testthat)
```

    ## 
    ## Attaching package: 'testthat'
    ## 
    ## The following object is masked from 'package:dplyr':
    ## 
    ##     matches
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     is_null
    ## 
    ## The following objects are masked from 'package:readr':
    ## 
    ##     edition_get, local_edition
    ## 
    ## The following object is masked from 'package:tidyr':
    ## 
    ##     matches

# Exercise 1&2 - Create a Function and Write a Document for this Function:

``` r
#' Summarize a Target Numeric Variable in a Data Frame by Specified Grouping Variables
#' 
#' This function calculates summary statistics for a specific numeric column in a data frame. It computes the 
#' mean, minimum, maximum, and standard deviation for each group defined by the specified grouping variables.
#' 
#' 
#' @param data A data frame containing the data to be summarized.
#' Named 'data' to indicate this part of function expects a data frame as input.
#' @param group_vars A character vector specifying the names of the columns to group by.
#' Named 'group_vars' to indicate these variables determine the grouping structure
#' @param target_var A single character string specifying the name of the numeric column to summarize.
#' Named 'target_var' to indicate this is the target variable need to be summarized.
#' @return A data frame containing the grouped summary statistics, including mean, minimum, maximum, and   
#' standard deviation of the target variable for each combination of grouping variables.




summarize_target <- function(data, group_vars, target_var){
  if (!is.data.frame(data)){
    stop("Sorry, your ",data," should be data frame.")
  }
  
  if (length(setdiff(group_vars, names(data))) != 0){
    stop("Sorry, all ",group_vars," should be column inside of your 'data'.")
  }
  
  if (!is.character(group_vars)){
    stop("Sorry, your ",group_vars," should be a character vector.")
  }
  
  if (!(target_var %in% names(data))) {
    stop(paste("Sorry, your ",target_var," does not exist in your 'data'."))
  }
  
  if (!is.character(target_var)|| length(target_var) != 1){
    stop("Sorry, your ",target_var," should be a single character vector.")
  }
  
  
  if (!is.numeric(data[[target_var]])) {
    stop("Sorry, the elements inside of your ",target_var," should be numeric.")
  }
  
  summarize_info <- data %>%
    group_by(across(all_of(group_vars))) %>%
    summarize(
      mean = mean(.data[[target_var]], na.rm = TRUE),
      min = min(.data[[target_var]], na.rm = TRUE),
      max = max(.data[[target_var]], na.rm = TRUE),
      sd = sd(.data[[target_var]], na.rm = TRUE)
    )
  
  return (summarize_info)
}
```

# Exercise 3 - Usage of Your Function

## Example 1:

``` r
discount_types <-  summarize_target(
  data = steam_games,
  group_vars = "types",
  target_var = "discount_price"
)

print(discount_types)
```

    ## # A tibble: 4 × 5
    ##   types   mean   min   max     sd
    ##   <chr>  <dbl> <dbl> <dbl>  <dbl>
    ## 1 app     51.9  0    963.  102.  
    ## 2 bundle  26.9  0.58 708.   35.9 
    ## 3 sub     11.4  0.49  60.0   9.08
    ## 4 <NA>    24.0 24.0   24.0  NA

We use the steam_games data here to compute summary statistics for the
discount_price variable group by types.

## Example 2:

``` r
group_error <-  summarize_target(
  data = steam_games,
  group_vars = "color",
  target_var = "discount_price"
)
```

    ## Error in summarize_target(data = steam_games, group_vars = "color", target_var = "discount_price"): Sorry, all color should be column inside of your 'data'.

The function will stop and display an error message indicating that the
specified grouping variable does not exist in the data frame.

## Example 3:

``` r
target_error <-  summarize_target(
  data = steam_games,
  group_vars = "types",
  target_var = "languages", error = TRUE
)
```

    ## Error in summarize_target(data = steam_games, group_vars = "types", target_var = "languages", : unused argument (error = TRUE)

The function will stop and display an error message indicating that the
target variable is not a numeric variable.

# Exercise 4 - Test the Function

``` r
# Define a simple data frame

Sim_data <- data.frame(
    type = c("A", "A", "B", "B"),
    value = c(10, 20, 30, 40),
    color = c("Blue", "red", "red", NA),
    count = c(5, 2, "NA", 4)
  )

 expected_summary <- tibble( type = c("A", "B"), mean = c(15, 35), min = c(10, 30), max = c(20, 40), sd = c(7.0710678, 7.0710678))

Test_Function <-  test_that("Testing multiplication function", {
  expect_equal(summarize_target(Sim_data, "type", "value"), expected_summary)
  expect_length(summarize_target(Sim_data, "color", "value"), 5)
  expect_error(summarize_target(Sim_data, "type", "color"), "Sorry, the elements inside of your color should be numeric.")
  expect_error(summarize_target(Sim_data, "type", "number"), "Sorry, your  number  does not exist in your 'data'.")
})
```

    ## Test passed 😀

In this part, I create a new simple data frame named Sim_data with clear
and easily calculable numeric values. I computed the mean, minimum,
maximum, and standard deviation by hand for each group to establish
expected summary statistics. Also, I created a series of formal tests to
verify the functionality of the summarize_target() function. These tests
check whether the function:

1.  Accurately computes summary statistics, when the target_var without
    missing values.
2.  Maintains the correct structure of the output even when the grouping
    variable contains NA values.
3.  Correctly identifies and reports errors when target_var is not a
    numeric vector.
4.  Correctly identifies and reports errors when a specified summary
    variable does not exist in the data frame.
