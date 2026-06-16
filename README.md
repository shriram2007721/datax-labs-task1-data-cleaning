# Task 1 — Data Cleaning & Preprocessing
**Data Analyst Internship | DataX Labs**

## Objective
Clean and preprocess a raw, real-world dataset — handling missing values, duplicates,
inconsistent formatting, and incorrect data types — to produce an analysis-ready dataset.

## Dataset
[Customer Personality Analysis](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis) (Kaggle)  
2,240 customers · 29 columns — demographics, spending behavior, and marketing campaign responses.

## Tools
Python, Pandas, NumPy, Matplotlib, Seaborn — run in a Kaggle Notebook (free).

## Before → After

| | Before | After |
|---|---|---|
| Rows | 2,240 | 2,237 |
| Columns | 29 | 28 |
| Missing values | 24 (income) | 0 |
| Duplicate rows | 3 | 0 |
| Junk categories | `Absurd`, `YOLO` in marital_status | Standardized to `Other` |
| Date column | String (DD-MM-YYYY) | datetime64 |
| Zero-variance cols | 2 (`z_costcontact`, `z_revenue`) | Dropped |
| Impossible ages | 3 records (born 1893–1900) | Removed |

## What I Did

1. **Loaded the data** — corrected for tab-separated format (common trap with this dataset)
2. **Explored structure** — used `.info()`, `.isnull().sum()`, `.duplicated().sum()` before changing anything
3. **Cleaned column headers** — lowercase, underscore-separated, whitespace-stripped
4. **Dropped 2 zero-variance columns** — `z_costcontact`, `z_revenue` (same value in every row)
5. **Filled 24 missing `income` values** — used median (robust to high-income outliers)
6. **Removed 3 duplicate rows** — also checked for duplicate customer IDs separately
7. **Standardized categorical text** — fixed casing; folded `Absurd`, `YOLO`, `Alone` → `Other`; standardized `2n Cycle` → `Master`
8. **Converted `dt_customer`** — from DD-MM-YYYY text to proper `datetime` type
9. **Engineered `age` column** — from `year_birth`; removed 3 records with impossible ages (>100)
10. **Flagged income outliers** — top 1% flagged for human review (not auto-removed)
11. **Built reusable pipeline** — entire process packaged into `clean_customer_data()` function
12. **Exported** — saved as `cleaned_customer_personality.csv`

## Creative Additions ★

Going beyond the basic task requirements:

- **Data Quality Score Card** — before/after metrics printed as a structured report
- **Spending by Age Group chart** — reveals that 40–60 age group spends the most
- **Campaign Response Rate analysis** — shows which campaigns performed best and response rate by education level
- **Income Outlier deep-dive** — boxplot + histogram separating normal vs high-income customers
- **Interview Q&A section** — all 8 task interview questions answered based on this project

## Key Insights Found During Cleaning

- Customers aged **40–60 are the highest spenders** — prime marketing target
- **Higher education correlates with higher campaign response rates**
- **3 customers had birth years in the 1800s** — clear data entry errors, removed
- **Top 1% income earners** may be VIP customers — flagged for human review rather than deleted

## Interview Q&A

**1. What are missing values and how do you handle them?**  
Missing values are NaNs — cells with no data. Here, `income` had 24 nulls. Filled with **median** (not mean) because income is right-skewed.

**2. How do you treat duplicate records?**  
Used `.drop_duplicates()` to remove exact full-row duplicates. Also checked for duplicate customer IDs using `.duplicated('id')`.

**3. Difference between dropna() and fillna()?**  
`dropna()` removes rows/columns with nulls entirely. `fillna()` replaces them with a value. Use `dropna()` when rows are unsalvageable; `fillna()` when you can estimate the missing value.

**4. What is outlier treatment and why is it important?**  
Outliers are extreme values that distort averages and model performance. We removed impossible birth years and flagged extreme incomes for human review — because they could be genuine VIP customers.

**5. Explain the process of standardizing data.**  
Standardized text with `.str.title()` and `.str.strip()`, folded junk categories into `Other`, and renamed all columns to lowercase underscore format.

**6. How do you handle inconsistent date formats?**  
`dt_customer` was stored as DD-MM-YYYY text. Converted using `pd.to_datetime(dayfirst=True)` so date arithmetic works correctly.

**7. What are common data cleaning challenges?**  
Wrong file delimiters, inconsistent casing, junk categorical values, dates as strings, zero-variance columns, and outliers needing human judgment.

**8. How can you check data quality?**  
Use `.info()`, `.isnull().sum()`, `.duplicated().sum()`, `.nunique()`, and `.describe()`. We packaged this into a reusable `data_quality_report()` function.

## Repo Structure
```
├── README.md
├── task1_data_cleaning.ipynb        # full commented notebook with creative additions
├── requirements.txt
```

## How to Run
1. Open the dataset page on Kaggle and click **New Notebook** (auto-attaches the data)
2. Import this notebook (`File → Import Notebook`)
3. Run all cells top to bottom

## Dependencies
```
pandas
numpy
matplotlib
seaborn
```
