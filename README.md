# STAT 545A: Mini Data Analysis Deliverable 1

**Student:** Zahra Alipour  
**Course:** STAT 545A - Exploratory Data Analysis  
**Institution:** University of British Columbia  
**Date:** December 2024

## Overview

This repository contains my Mini Data Analysis Deliverable 1 for STAT 545A. The analysis explores the `cancer_sample` dataset from the `datateachr` package to understand diagnostic measurements for breast cancer classification.

## Repository Contents

### Main Files
- `mini-data-analysis1.Rmd` - Source R Markdown document
- `mini-data-analysis1.md` - Knitted GitHub document output
- `mini-data-analysis1_files/` - Directory containing all generated plots

### Generated Plots
- `exercise1-diagnosis-distribution-1.png` - Distribution of cancer diagnosis
- `exercise2-missing-values-1.png` - Missing data visualization (matrix view)
- `exercise2-missing-values-alt-1.png` - Missing data by variable
- `exercise3-filtering-summary-1.png` - Box plot comparing radius measurements
- `exercise4-relationships-1.png` - Scatter plot showing variable relationships

## Analysis Summary

### Task 1: Dataset Exploration
- Explored 4 datasets from `datateachr` package
- Selected `cancer_sample` dataset (569 observations, 32 variables)
- Provided clear justification for dataset choice

### Task 2: Exploratory Data Analysis
Performed 4 exploration exercises using `dplyr` and `ggplot2`:
1. **Distribution Analysis** - Examined class balance of malignant vs benign cases
2. **Missing Values Analysis** - Verified complete dataset with no missing values
3. **Filtering and Summary Statistics** - Compared radius measurements between diagnoses
4. **Variable Relationships** - Explored correlation between radius and texture measurements

### Task 3: Research Questions for Milestone 2
Developed 4 research questions focusing on:
- Predictive modeling using mean measurements
- Feature selection for optimal classification
- Variability differences between diagnoses
- Clustering analysis for tumor subtypes

## Technical Features

- **Modern R Practices**: Uses latest `dplyr` and `tidyr` functions
- **Professional Visualizations**: Incorporates `naniar` package for missing data analysis
- **Reproducible Analysis**: Includes `sessionInfo()` for complete reproducibility
- **Clean Code**: Well-documented with clear explanations for each analysis step

## How to Engage with This Repository

1. **View the Analysis**: Open `mini-data-analysis1.md` to see the complete knitted output
2. **Run the Code**: Use `mini-data-analysis1.Rmd` in RStudio to reproduce the analysis
3. **Review Plots**: Check the `mini-data-analysis1_files/figure-gfm/` directory for all visualizations
4. **Check Reproducibility**: All code runs without errors and produces consistent results

## Dependencies

```r
library(datateachr)
library(tidyverse)
library(glue)
library(naniar)  # Optional, with graceful fallback
```

## Submission

This repository contains the complete Milestone 1 submission for STAT 545A Mini Data Analysis Project. All requirements have been met including dataset exploration, analysis exercises, and research question development for future milestones.

---

*For questions or issues, please refer to the course materials or contact the instructor.*