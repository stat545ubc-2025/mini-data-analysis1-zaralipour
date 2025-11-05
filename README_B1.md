# Assignment B1: Making a Function in R

**Student:** Zahra Alipour  
**Course:** STAT 545B  
**Assignment:** B1 - Making a Function in R  
**Date:** November 2025

## Overview

This repository contains my submission for Assignment B1, which focuses on creating, documenting, testing, and using a custom R function. The function `summarise_by_group()` was created to streamline a common data analysis workflow from my STAT 545A work with the `cancer_sample` dataset.

## Repository Contents

- **`assignment_b1.Rmd`**: The main R Markdown file containing:
  - Function definition with roxygen2 documentation
  - Multiple usage examples
  - Formal tests using the `testthat` package
  - Detailed explanations and justifications

- **`README_B1.md`**: This file, providing an overview of the repository

## Function Description

The `summarise_by_group()` function calculates summary statistics (range, mean, median, and standard deviation) for a numerical variable across groups of a categorical variable. This addresses a repeated pattern in data analysis where you need to compare summary statistics across different groups.

### Key Features

- Calculates range, mean, median, and standard deviation by group
- Handles missing values with configurable `na.rm` parameter
- Returns consistent tibble output
- Provides clear error messages for invalid inputs
- Works with any data frame and variable combination

## Usage

To use this function:

1. Load the required libraries:
```r
library(tidyverse)
library(datateachr)
```

2. Use the function with your data:
```r
summarise_by_group(cancer_sample, diagnosis, radius_mean)
```

## Dependencies

The assignment requires the following R packages:
- `tidyverse` (for data manipulation and visualization)
- `testthat` (for testing)
- `datateachr` (for the example dataset)

## How to Run

1. Open `assignment_b1.Rmd` in RStudio
2. Install required packages if needed
3. Knit the document to generate the GitHub markdown output
4. All code chunks should run without errors

## Testing

The function includes comprehensive tests that verify:
- Correct calculation of statistics
- Proper handling of missing values
- Edge cases (empty data frames)
- Consistent output structure
- Appropriate error handling

All tests pass successfully.

## Assignment Requirements Met

✅ **Exercise 1**: Function created with no magic numbers, flexible inputs, consistent output  
✅ **Exercise 2**: Complete roxygen2 documentation with title, description, @param, and @return tags  
✅ **Exercise 3**: Multiple examples demonstrating different use cases  
✅ **Exercise 4**: Formal tests using testthat with at least 3 non-redundant test cases

## Notes

- The function is designed to be self-contained and does not rely on the working environment
- All parameters are configurable - no hardcoded values
- Error handling provides informative messages
- The function follows tidyverse conventions and style

