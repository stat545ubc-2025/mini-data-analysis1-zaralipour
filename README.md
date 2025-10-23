# STAT 545A: Mini Data Analysis Project

**Student:** Zahra Alipour  
**Course:** STAT 545A - Exploratory Data Analysis  
**Institution:** University of British Columbia  
**Date:** December 2025

## Overview

This repository contains my Mini Data Analysis Project for STAT 545A, including both Milestone 1 and Milestone 2. The analysis explores the `cancer_sample` dataset from the `datateachr` package to understand diagnostic measurements for breast cancer classification using statistical analysis and machine learning techniques.

## Repository Structure

### Main Folders
- `Milestone1/` - Contains all files for Milestone 1
  - `mini-data-analysis1.Rmd` - Source R Markdown document
  - `mini-data-analysis1.md` - Knitted GitHub document output
  - `mini-data-analysis1_files/` - Directory containing all generated plots
  - `README.md` - Description of Milestone 1 contents
- `Milestone2/` - Contains all files for Milestone 2
  - `mini-data-analysis2.Rmd` - Source R Markdown document (instructor template)
  - `mini-data-analysis2.md` - Knitted GitHub document output
  - `mini-data-analysis2_files/` - Directory containing all generated plots
  - `README.md` - Description of Milestone 2 contents
- `output/` - Directory containing CSV and RDS files from Milestone 2
  - `radius_summary.csv` - Summary statistics
  - `rf_model.rds` - Saved Random Forest model
  - `README.md` - Description of output files

### Generated Plots
- `exercise1-diagnosis-distribution-1.png` - Distribution of cancer diagnosis
- `exercise2-missing-values-1.png` - Missing data visualization (matrix view)
- `exercise2-missing-values-alt-1.png` - Missing data by variable
- `exercise3-filtering-summary-1.png` - Box plot comparing radius measurements
- `exercise4-relationships-1.png` - Scatter plot showing variable relationships

## Analysis Summary

### Milestone 1: Exploratory Data Analysis
- Explored 4 datasets from `datateachr` package
- Selected `cancer_sample` dataset (569 observations, 32 variables)
- Performed 4 exploration exercises using `dplyr` and `ggplot2`
- Developed research questions for advanced analysis

### Milestone 2: Statistical Analysis and Machine Learning
- **Research Questions**: 4 comprehensive questions addressing prediction, feature selection, variability, and clustering
- **Data Wrangling**: Data exploration, tidying, and cleaning decisions using instructor template structure
- **Statistical Analysis**: T-tests, ANOVA, correlation analysis using broom package
- **Machine Learning**: Random Forest models (97.5% accuracy), feature selection analysis, clustering analysis (3 distinct clusters)
- **File Operations**: CSV and RDS file writing with robustness and reproducibility
- **Instructor Template**: Follows exact template structure with 8 tasks, data tidying, modeling, and file operations
- **Key Results**: All measurements show highly significant differences between malignant and benign cases

## Technical Features

- Uses modern `dplyr` and `tidyr` functions for data manipulation
- Includes `naniar` package for missing data analysis
- Contains `sessionInfo()` for reproducibility
- Well-documented code with explanations for each analysis step

## How to Use This Repository

1. **Milestone 1**: Open `Milestone1/mini-data-analysis1.md` to see the complete exploratory analysis
2. **Milestone 2**: Open `Milestone2/mini-data-analysis2.md` to see the complete statistical analysis, or `Milestone2/mini-data-analysis2.Rmd` in RStudio to run the code
3. Check the `Milestone1/mini-data-analysis1_files/figure-gfm/` directory for all plots from Milestone 1
4. Check the `Milestone2/mini-data-analysis2_files/figure-gfm/` directory for all plots from Milestone 2
5. Check the `output/` directory for CSV and RDS files from Milestone 2
6. All code should run without errors

## Dependencies

```r
# Core packages
library(datateachr)
library(tidyverse)
library(broom)
library(glue)

# Additional packages for Milestone 2
library(caret)
library(randomForest)
library(naniar)  # Optional, with graceful fallback
```

## Submission

This repository contains the complete Mini Data Analysis Project for STAT 545A, including both Milestone 1 and Milestone 2. All requirements have been met including dataset exploration, statistical analysis, machine learning, and comprehensive research question development.

---

*For questions or issues, please contact me at: zaralipour@gmail.com*