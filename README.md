# Data Cleaning in MySQL — Tech Layoffs Dataset

## Overview
SQL data cleaning project using a real-world dataset of tech company layoffs.
The goal was to prepare raw data for analysis by removing duplicates,
standardizing inconsistent values, handling nulls, and fixing data types.

## Dataset
- **Source**: [layoffs.csv](https://github.com/AlexTheAnalyst/MySQL-YouTube-Series/blob/main/layoffs.csv)
- **Content**: Tech company layoffs — company name, location, industry,
  number laid off, percentage, funding stage, country, date

## Steps Performed

### 1. Remove Duplicates
- Created a staging table (`layoffs_staging`) to avoid touching raw data
- Used `ROW_NUMBER() OVER(PARTITION BY ...)` to identify exact duplicates
- Created a second staging table (`layoffs_staging2`) with a `row_num` column
  to be able to delete duplicates (CTEs don't support DELETE directly in MySQL)

### 2. Standardize Data
- Trimmed whitespace from `company` names with `TRIM()`
- Unified inconsistent industry names (e.g. `Crypto Currency`, `CryptoCurrency` → `Crypto`)
- Removed trailing periods from `country` values (e.g. `United States.` → `United States`)
- Converted `date` column from TEXT to proper DATE type using `STR_TO_DATE()`

### 3. Handle Null / Blank Values
- Converted empty strings in `industry` to NULL for consistency
- Used a self-JOIN to populate missing `industry` values from other rows
  of the same company that had the value filled in
- Removed rows where both `total_laid_off` and `percentage_laid_off` were NULL
  (no useful data could be extracted from them)

### 4. Remove Unnecessary Columns
- Dropped the `row_num` helper column after duplicates were removed

## Files
```
data-cleaning-layoffs/
├── README.md
├── data/
│   └── layoffs.csv          # raw dataset
└── scripts/
    └── data_cleaning.sql    # full cleaning script
```

## Tools
- MySQL 8.0
- MySQL Workbench
