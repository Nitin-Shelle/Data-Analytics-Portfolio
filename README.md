SQL Project: World Layoffs Data Cleaning & Transformation
This repository contains SQL scripts and documentation for cleaning, standardizing, and refining a global layoffs dataset. The main script (main.sql) performs four key stages: removing duplicates, standardizing data, handling null/blank values, and removing unnecessary columns.

1. Removing Duplicates
Purpose of layoffs_staging2 table
A second staging table (layoffs_staging2) was created to hold all records along with a computed row_num column. This extra column tracks each row’s rank within groups of identical data, enabling precise removal of duplicate rows while preserving one “original” record per group.

Deleting duplicates using CTE and window functions

Loaded all rows into layoffs_staging2.

Applied a ROW_NUMBER() window function partitioned by key fields (company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions) to assign each record a unique sequence number (row_num) within its group.

Used a Common Table Expression (CTE) to filter where row_num > 1, identifying duplicates.

Executed DELETE FROM layoffs_staging2 WHERE row_num > 1 to remove all but the first occurrence in each group.

2. Standardizing the Data
Trimming whitespace in text columns

Applied UPDATE ... SET company = TRIM(company) to remove leading/trailing spaces from the company names.

Handling trailing periods in country names

Some entries for “United States.” included a trailing period. The function TRIM(TRAILING '.' FROM country) removed only the trailing dot, preserving the rest of the country string intact.

Converting date strings to proper DATE type

Used STR_TO_DATE(date, '%m/%d/%Y') to parse text dates in MM/DD/YYYY format into MySQL’s internal DATE representation.

Updated the date column with parsed values via UPDATE ... SET date = STR_TO_DATE(date, '%m/%d/%Y').

Altering column data type

Once all date values were parsed correctly, executed ALTER TABLE layoffs_staging2 MODIFY COLUMN date DATE to change the column’s type from TEXT to DATE, enforcing proper date semantics in the table schema.

3. Checking Null & Blank Values
Identifying rows with missing metrics

Queried WHERE total_laid_off IS NULL AND percentage_laid_off IS NULL to find records lacking both key layoff metrics.

Populating missing industry values via self-join

Employed an UPDATE with a self-join on company and location, matching rows where industry was blank or NULL to other rows for the same company/location that had a valid industry. This ensured all rows for a given entity/location inherited a consistent industry label.

Exclusion of numeric metrics

Chose not to auto-populate total_laid_off or funds_raised_millions via joins, to avoid introducing potentially incorrect numerical estimates. These metrics were left blank when originally missing, preserving data accuracy and signaling genuine data gaps.

4. Removing Unnecessary Columns
Deleting rows with no layoff metrics

After data standardization, executed DELETE FROM layoffs_staging2 WHERE total_laid_off IS NULL AND percentage_laid_off IS NULL to drop records that lacked all layoff measurements.

Dropping the auxiliary row_num column

Once duplicate removal was complete, removed the now-obsolete row_num column via ALTER TABLE layoffs_staging2 DROP COLUMN row_num, leaving the table with only meaningful data fields for downstream analysis.
