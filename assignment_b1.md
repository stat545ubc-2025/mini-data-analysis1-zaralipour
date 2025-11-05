Assignment B1: Making a Function in R
================
Zahra Alipour (Student Number: 65174591)
2025-11-04

# Assignment B1: Making a Function in R

**Student:** Zahra Alipour  
**Student Number:** 65174591

This document contains my submission for Assignment B1, which
demonstrates creating, documenting, testing, and using a custom R
function.

## Exercise 1: Make a Function

In my STAT 545A analysis of the `cancer_sample` dataset, I repeatedly
calculated summary statistics grouped by categorical variables. For
example, I would calculate the mean, median, standard deviation, min,
max, and range of numerical variables like `radius_mean` across
different `diagnosis` groups.

To streamline this common workflow, I’ve created a function called
`summarise_by_group()` that calculates range, mean, and two additional
summary statistics (median and standard deviation) for a numerical
variable across groups of a categorical variable.

``` r
#' Calculate Summary Statistics by Group
#' 
#' This function calculates the range, mean, median, and standard deviation 
#' of a numerical variable across the groups of a categorical variable.
#' 
#' @param data A data frame containing the variables of interest. Named "data" 
#'   following the tidyverse convention used in functions like `filter()` and 
#'   `mutate()`.
#' @param group_var The name of the categorical variable to group by (unquoted). 
#'   Named "group_var" to clearly indicate this is the grouping variable, 
#'   distinguishing it from the numerical variable being summarized.
#' @param num_var The name of the numerical variable to summarize (unquoted). 
#'   Named "num_var" as a short, clear abbreviation for "numerical variable" 
#'   that indicates the type of variable being summarized.
#' @param na.rm Logical indicating whether to remove missing values when 
#'   calculating statistics. Default is TRUE. Named "na.rm" following the 
#'   standard R convention used in functions like `mean()`, `sd()`, and `median()`.
#' 
#' @return A tibble with one row per group, containing the following columns:
#'   - The grouping variable with its values
#'   - count: total number of observations in each group (including NAs)
#'   - n_non_missing: number of non-missing observations for the numerical variable
#'   - mean: mean of the numerical variable (returns NA if all values are NA)
#'   - median: median of the numerical variable (returns NA if all values are NA)
#'   - sd: standard deviation of the numerical variable (returns NA if all values are NA)
#'   - min: minimum value
#'   - max: maximum value
#'   - range: difference between max and min
#' 
#' @examples
#' library(datateachr)
#' summarise_by_group(cancer_sample, diagnosis, radius_mean)
summarise_by_group <- function(data, group_var, num_var, na.rm = TRUE) {
  # Validate inputs
  if (!is.data.frame(data)) {
    stop("data must be a data frame")
  }
  
  # Capture the variable names as strings for use in dplyr functions
  group_var_name <- rlang::as_name(rlang::enquo(group_var))
  num_var_name <- rlang::as_name(rlang::enquo(num_var))
  
  # Check if variables exist in the data frame
  if (!group_var_name %in% names(data)) {
    stop(paste0("Grouping variable '", group_var_name, "' not found in data"))
  }
  if (!num_var_name %in% names(data)) {
    stop(paste0("Numerical variable '", num_var_name, "' not found in data"))
  }
  
  # Check if num_var is numeric
  if (!is.numeric(data[[num_var_name]])) {
    stop(paste0("'", num_var_name, "' must be numeric"))
  }
  
  # Calculate summary statistics
  result <- data %>%
    group_by({{ group_var }}) %>%
    summarise(
      count = n(),
      n_non_missing = sum(!is.na({{ num_var }})),
      min = {
        v <- suppressWarnings(min({{ num_var }}, na.rm = na.rm))
        if (is.infinite(v) || sum(!is.na({{ num_var }})) == 0) NA_real_ else v
      },
      max = {
        v <- suppressWarnings(max({{ num_var }}, na.rm = na.rm))
        if (is.infinite(v) || sum(!is.na({{ num_var }})) == 0) NA_real_ else v
      },
      mean = {
        m <- mean({{ num_var }}, na.rm = na.rm)
        if (is.nan(m)) NA_real_ else m
      },
      median = {
        m <- median({{ num_var }}, na.rm = na.rm)
        if (length(m) == 0 || is.nan(m)) NA_real_ else m
      },
      sd = {
        s <- sd({{ num_var }}, na.rm = na.rm)
        if (is.nan(s)) NA_real_ else s
      },
      range = if (is.na(min) | is.na(max)) NA_real_ else max - min,
      .groups = "drop"
    )
  
  return(result)
}
```

**Design decisions:**

1.  **Function name**: I chose `summarise_by_group()` because it clearly
    describes what the function does - it summarises data by groups.

2.  **Parameter names**:

    - `data`: Standard name in tidyverse functions (e.g., `filter()`,
      `mutate()`)
    - `group_var`: Descriptive name indicating this is the grouping
      variable
    - `num_var`: Short for “numerical variable”, clearly indicating the
      variable to summarize
    - `na.rm`: Standard parameter name in R statistical functions (e.g.,
      `mean()`, `sd()`)

3.  **Flexibility**: The function accepts any data frame and any
    grouping/numerical variable pair, making it flexible for different
    datasets.

4.  **Consistent output**: Always returns a tibble with the same
    structure, regardless of input.

5.  **Error handling**: Validates that inputs are data frames and that
    specified variables exist.

6.  **No magic numbers**: All calculations use standard statistical
    functions with configurable parameters.

## Exercise 2: Document your Function

The function above includes comprehensive roxygen2 documentation with: -
A title: “Calculate Summary Statistics by Group” - A description
explaining what the function does - `@param` tags for each argument with
justification for naming - A `@return` tag describing the output
structure - A `@examples` tag showing usage

## Exercise 3: Include Examples

Here are several examples demonstrating the usage of
`summarise_by_group()`:

### Example 1: Basic usage with cancer_sample dataset

``` r
# Calculate summary statistics for radius_mean grouped by diagnosis
radius_summary <- summarise_by_group(cancer_sample, diagnosis, radius_mean)
print(radius_summary)
```

    ## # A tibble: 2 × 9
    ##   diagnosis count n_non_missing   min   max  mean median    sd range
    ##   <chr>     <int>         <int> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 B           357           357  6.98  17.8  12.1   12.2  1.78  10.9
    ## 2 M           212           212 11.0   28.1  17.5   17.3  3.20  17.2

This shows that malignant tumors have larger radius measurements on
average than benign tumors. The output displays the mean values for each
group, which can be seen in the printed tibble above.

### Example 2: Using with a different numerical variable

``` r
# Calculate summary statistics for area_mean grouped by diagnosis
area_summary <- summarise_by_group(cancer_sample, diagnosis, area_mean)
print(area_summary)
```

    ## # A tibble: 2 × 9
    ##   diagnosis count n_non_missing   min   max  mean median    sd range
    ##   <chr>     <int>         <int> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 B           357           357  144.  992.  463.   458.  134.  849.
    ## 2 M           212           212  362. 2501   978.   932   368. 2139.

This demonstrates that the function works with different numerical
variables from the same dataset.

### Example 3: Handling missing values

``` r
# Create a dataset with missing values
data_with_na <- cancer_sample %>%
  mutate(radius_mean = ifelse(row_number() %% 10 == 0, NA, radius_mean))

# With na.rm = TRUE (default)
summary_with_na <- summarise_by_group(data_with_na, diagnosis, radius_mean, na.rm = TRUE)
print(summary_with_na)
```

    ## # A tibble: 2 × 9
    ##   diagnosis count n_non_missing   min   max  mean median    sd range
    ##   <chr>     <int>         <int> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 B           357           322  6.98  17.8  12.2   12.2  1.79  10.9
    ## 2 M           212           191 11.0   28.1  17.5   17.4  3.19  17.2

``` r
# With na.rm = FALSE (shows how NA affects results)
summary_with_na_false <- summarise_by_group(data_with_na, diagnosis, radius_mean, na.rm = FALSE)
print(summary_with_na_false)
```

    ## # A tibble: 2 × 9
    ##   diagnosis count n_non_missing   min   max  mean median    sd range
    ##   <chr>     <int>         <int> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 B           357           322    NA    NA    NA     NA    NA    NA
    ## 2 M           212           191    NA    NA    NA     NA    NA    NA

This demonstrates how the function handles missing values when
`na.rm = TRUE` vs `na.rm = FALSE`.

### Example 4: Using with a different grouping variable

``` r
# Create a categorical variable from a numerical one
cancer_sample_with_size <- cancer_sample %>%
  mutate(
    tumor_size = case_when(
      radius_mean < 12 ~ "Small",
      radius_mean >= 12 & radius_mean < 18 ~ "Medium",
      radius_mean >= 18 ~ "Large"
    )
  )

# Summarise texture_mean by tumor_size
texture_by_size <- summarise_by_group(cancer_sample_with_size, tumor_size, texture_mean)
print(texture_by_size)
```

    ## # A tibble: 3 × 9
    ##   tumor_size count n_non_missing   min   max  mean median    sd range
    ##   <chr>      <int>         <int> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ## 1 Large         92            92 14.3   32.5  21.9   21.5  3.61  18.1
    ## 2 Medium       308           308 10.4   39.3  19.2   18.8  4.33  28.9
    ## 3 Small        169           169  9.71  33.8  18.0   17.5  3.98  24.1

This shows the function works with any categorical grouping variable,
not just the original variables in the dataset. Note that if
`radius_mean` is `NA` for any row, the `case_when()` will set
`tumor_size = NA`, which is handled correctly by the function.

### Example 5: Error handling

``` r
# This should produce an error - variable doesn't exist
summarise_by_group(cancer_sample, diagnosis, nonexistent_var)
```

    ## Error in summarise_by_group(cancer_sample, diagnosis, nonexistent_var): Numerical variable 'nonexistent_var' not found in data

``` r
# This should produce an error - not a data frame
summarise_by_group(c(1, 2, 3), diagnosis, radius_mean)
```

    ## Error in summarise_by_group(c(1, 2, 3), diagnosis, radius_mean): data must be a data frame

These examples demonstrate that the function provides helpful error
messages when given invalid inputs.

## Exercise 4: Test the Function

I’ve written formal tests using the `testthat` package to ensure the
function works correctly:

``` r
# Test 1: Function works with numeric vector with no NAs
test_that("Function works with numeric vector with no NAs", {
  result <- summarise_by_group(cancer_sample, diagnosis, radius_mean)
  
  # Check structure
  expect_true(is.data.frame(result))
  expect_true(nrow(result) > 0)  # Should have at least one group
  expect_true(all(c("diagnosis", "count", "n_non_missing", "mean", "median", "sd", "min", "max", "range") %in% names(result)))
  
  # Check that statistics are calculated correctly
  expect_true(all(result$count > 0))
  expect_true(all(result$n_non_missing > 0))
  expect_true(all(result$n_non_missing <= result$count))
  expect_true(all(is.finite(result$mean)))
  expect_true(all(result$max >= result$min))
  expect_true(all(is.finite(result$range)))
  expect_true(all(result$range == result$max - result$min))
})
```

    ## Test passed 🥳

``` r
# Test 2: Function handles NAs correctly
test_that("Function handles NAs correctly", {
  # Create data with NAs
  data_with_na <- cancer_sample %>%
    mutate(radius_mean = ifelse(row_number() <= 10, NA, radius_mean))
  
  # With na.rm = TRUE, should work and exclude NAs
  result_with_na <- summarise_by_group(data_with_na, diagnosis, radius_mean, na.rm = TRUE)
  expect_true(all(is.finite(result_with_na$mean) | is.na(result_with_na$mean)))
  expect_true(all(is.finite(result_with_na$median) | is.na(result_with_na$median)))
  expect_true(all(result_with_na$n_non_missing <= result_with_na$count))
  
  # With na.rm = FALSE, should return NA for mean/median when NAs present
  result_without_na <- summarise_by_group(data_with_na, diagnosis, radius_mean, na.rm = FALSE)
  # Should have some NA values when na.rm = FALSE and NAs are present
  expect_true(any(is.na(result_without_na$mean)))
  
  # Test all-NA group: should return NA (not NaN) for statistics
  data_all_na <- cancer_sample %>%
    mutate(radius_mean = NA_real_) %>%
    slice_head(n = 10)
  
  result_all_na <- summarise_by_group(data_all_na, diagnosis, radius_mean, na.rm = TRUE)
  # Should return NA (not NaN or Inf) when all values are NA
  # The function converts NaN to NA, and Inf/-Inf to NA for min/max
  expect_true(all(is.na(result_all_na$mean)))
  expect_true(all(is.na(result_all_na$median)))
  expect_true(all(is.na(result_all_na$sd)))
  expect_true(all(is.na(result_all_na$min)))
  expect_true(all(is.na(result_all_na$max)))
  expect_true(all(is.na(result_all_na$range)))
})
```

    ## Test passed 🥇

``` r
# Test 3: Function works with empty vector (length 0)
test_that("Function works with empty data frame", {
  empty_df <- cancer_sample[0, ]  # Empty data frame
  
  result <- summarise_by_group(empty_df, diagnosis, radius_mean)
  
  # Should return empty tibble with correct structure
  expect_true(is.data.frame(result))
  expect_equal(nrow(result), 0)
  expect_true(all(c("diagnosis", "count", "n_non_missing", "mean", "median", "sd", "min", "max", "range") %in% names(result)))
})
```

    ## Test passed 🥇

``` r
# Test 4: Function returns consistent output structure
test_that("Function returns consistent output structure", {
  result1 <- summarise_by_group(cancer_sample, diagnosis, radius_mean)
  result2 <- summarise_by_group(cancer_sample, diagnosis, texture_mean)
  result3 <- summarise_by_group(cancer_sample, diagnosis, area_mean)
  
  # All results should have the same column names
  expect_equal(names(result1), names(result2))
  expect_equal(names(result2), names(result3))
  
  # All should be tibbles
  expect_true(is.data.frame(result1))
  expect_true(is.data.frame(result2))
  expect_true(is.data.frame(result3))
})
```

    ## Test passed 🎊

``` r
# Test 5: Function provides appropriate error messages
test_that("Function provides appropriate error messages", {
  # Non-existent variable
  expect_error(
    summarise_by_group(cancer_sample, diagnosis, fake_variable),
    "not found in data"
  )
  
  # Non-data frame input
  expect_error(
    summarise_by_group(c(1, 2, 3), diagnosis, radius_mean),
    "must be a data frame"
  )
  
  # Non-numeric variable
  expect_error(
    summarise_by_group(cancer_sample, diagnosis, diagnosis),
    "must be numeric"
  )
})
```

    ## Test passed 😀

All tests pass, confirming that the function works correctly across
different scenarios.

## Summary

This assignment demonstrates: - Creating a reusable function that
encapsulates a common data analysis workflow - Comprehensive
documentation using roxygen2 - Multiple examples showing different use
cases - Formal testing to ensure correctness and reliability

The `summarise_by_group()` function is flexible, well-documented, and
robust, making it a useful addition to any data analysis toolkit.
