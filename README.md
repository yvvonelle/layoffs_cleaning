# 🧹 SQL Data Cleaning: Global Layoffs Dataset

## 📌 Project Overview
Data is rarely clean. This project demonstrates a comprehensive **ETL (Extract, Transform, Load)** process using SQL to take a messy, real-world dataset of global layoffs and transform it into a high-quality asset for analysis.

The goal was to move data from a raw, unreliable state to a "Production-Ready" staging table by resolving duplicates, formatting inconsistencies, and missing values.

---

## 🛠️ The Cleaning Pipeline
I followed a structured 6-step framework to ensure data integrity:

### 1. Staging & Architecture
* **Action:** Created `layoffs_staging` (to preserve raw data) and `layoffs_staging2` (for transformation).
* **Reasoning:** Always work on a staging table to prevent accidental loss of original source data.

### 2. Deduplication (The Row Number Method)
* **Logic:** Used `ROW_NUMBER()` partitioned by all columns to find exact duplicates.
* **Technique:** Wrapped the logic in a **CTE (Common Table Expression)** to identify and delete rows where the count was greater than 1.

### 3. Text Standardization
* **Trimming:** Removed white spaces from `company` names.
* **Categorization:** Grouped inconsistent industry labels (e.g., merging "Crypto Currency" and "CryptoNext" into "Crypto").
* **Geography Fixes:** Cleaned trailing punctuation in the `country` column (e.g., "United States." → "United States").

### 4. Date Standardization
* **Transformation:** Converted the `date` column from a `TEXT` string to a proper `DATE` format using `STR_TO_DATE()`.
* **Data Typing:** Altered the table schema to change the column type permanently to enable time-series analysis.

### 5. Handling Missing Values (Data Enrichment)
* **Null Imputation:** Populated missing `industry` values by performing a **Self-Join**—looking up other records of the same company to find the correct industry.
* **Purging:** Removed records where both `total_laid_off` and `percentage_laid_off` were NULL, as they provided no actionable insight for layoff analysis.

### 6. Final Schema Optimization
* **Action:** Dropped helper columns (like `row_num`) used during the cleaning process to keep the final table lean and efficient.

---

## 🧮 Key SQL Techniques Used
* **Window Functions:** `ROW_NUMBER() OVER(PARTITION BY...)`
* **CTEs:** For readable, multi-step transformations.
* **String Functions:** `TRIM()`, `TRAILING`, `UPDATE`.
* **Joins:** Self-Joins for data imputation.
* **DDL & DML:** `CREATE TABLE`, `ALTER TABLE`, `UPDATE`, `DELETE`.

---

## 📈 Final Outcome
**Cleaned Table:** `layoffs_staging2`

The result is a dataset that is:
1.  **Unique:** Zero duplicate entries.
2.  **Standardized:** Consistent naming conventions across Company, Industry, and Country.
3.  **Time-Ready:** Dates are correctly typed for chronological filtering.
4.  **Reliable:** Minimized Null values through logical data enrichment.



---

## 🚀 How to Run the Script
1. Import the raw `layoffs.csv` into your SQL environment.
2. Run the provided cleaning script sequentially.
3. The final cleaned data will be available in the `layoffs_staging2` table.
