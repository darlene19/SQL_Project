What are your risk areas? Identify and describe them.

1. There were a lot of null values all throughout the dataset. Null values are not always necessarily equals to zero, it can be a missing or unknown value. This made it hard to decide on whether to keep them or delete them from the dataset. Deleting them may result in a loss of valuable data, on the other hand, keeping them may lead to a biased or inaccurate analysis. For this project, I decided to keep them as a distinct category as I do not have enough informaton on what exactly the null values represent in this dataset.

2. In analytics table, there was a negative value for the units_sold which indicates an erroneous situation. It could be a result of human data entry mistakes, or a return, refund or void transaction. It is important to further investigate the occurence of this negative value to take the necessary actions, such as deleting or keeping it from the dataset.

SQL Query:
```
SELECT units_sold
FROM analytics
WHERE units_sold < 0
```
The query gave record with the negative value.

3. In products table, checking if product_sku have no duplicate values since primary key should be unique.

SQL Query:
```
SELECT product_sku, COUNT(product_sku) as pd_count
FROM products
GROUP BY product_sku
HAVING COUNT(product_sku) > 1
```

The query yields no result as expected.

4. Checking referential integrity between products and sales_report table to ensure the primary key-foreign key relationship between tables remain consistent and valid using product_sku.

SQL Query:
```
SELECT 'sales_report' as table_name,
	COUNT(*) as row_count,
	'product_sku' as key_name,
	COUNT(DISTINCT('product_sku')) AS distinct_key_count
FROM sales_report

UNION ALL

SELECT 'products' AS table_name,
	COUNT(*) AS row_count,
	'product_sku' AS key_name,
	COUNT(DISTINCT(product_sku)) AS distinct_key_count
FROM products

--cte to QA distinct value count of product_sku

WITH cte AS (
	SELECT 'sales_report' AS table_name,
	COUNT(*) AS row_count,
	'product_sku' AS key_name,
	COUNT(DISTINCT(product_sku)) AS distinct_product_sku_count
	FROM products),
	
QA_raw AS (
	SELECT COUNT(DISTINCT(distinct_product_sku_count)) AS test_result FROM cte)
	
SELECT 'referential_integrity_test' as QA_test,
	CASE
		WHEN test_result = 1 THEN 'Pass'
		ELSE 'Fail'
	END AS QA_result
FROM QA_raw
```

The QA_result showed Pass.


Future Goals

In general, there were a lot of risk areas from the data provided such as duplicate values, null values, lack of information about the data itself such as information about the description of columns in each table. If given more time, I would like to work more on the following:

1. Create 2 or more tables from all_sessions table to obtain more consistent data. Creating a separate table for visitor information and visitor transaction can improve the accuracy and reliability of the data. 

2. Regarding the null values, I want to perform a data completeness check by calculating the percentage of null values in each column. If possible, I will replace them with the mean, median, or mode of the non-null values in the same column.

3. Develop SQL queries to obtain statistical analysis for each column for each table, such as minimum, maximum, average, etc. to help detect any potential data issues and outliers, which can indicate possible data entry errors. One example was the negative value for the units_sold in analytics table. Once I figure out the specific reason for its occurence, then I can better decide and make the necessary adjustments.

4. Create some kind of visualization to better analyze and understand the patterns and relationships between the data as a whole, which is not always obvious when just looking at numbers from tables. For example, include the date on the analysis. Are there patterns and trends on when visitors are purchasing certain products? Which month or year gives the highest revenue? By answering these kind of questions we can find more insights from the analyzed data.

