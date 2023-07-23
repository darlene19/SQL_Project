What are your risk areas? Identify and describe them.

1. In analytics table, there was a negative value for the units_sold which indicates an erroneous situation. It could be a result of human data entry mistakes, or a return, refund or void transaction. It is important to further investigate the occurence of this negative value to take the necessary actions, such as deleting or keeping it from the dataset.

SQL Query:
```
SELECT units_sold
FROM analytics
WHERE units_sold < 0
```
The query gave one record with a value of -89.

2. Checking for any missing or null values. 
   
SQL Query:
```
SELECT product_sku 
FROM products
WHERE product_sku IS null
```

The query yields no result. 

3. In products table, checking if product_sku have no duplicate values since primary key should be unique.

SQL Query:
```
SELECT product_sku, COUNT(product_sku) as pd_count
FROM products
GROUP BY product_sku
HAVING COUNT(product_sku) >1
```

The query yields no result as expected.

4. Checking referential integrity between products and sales_report table to ensure the primary key-foreign key relationship between tables remain consistent and valid using product_sku.

SQL Query:
```
SELECT 'sales_report' as table_name,
	COUNT(*) as count_rows,
	'product_sku' as key_name,
	COUNT(DISTINCT('product_sku')) AS distinct_key_count
FROM sales_report

UNION ALL

SELECT 'products' AS table_name,
	COUNT(*) AS count_rows,
	'product_sku' AS key_name,
	COUNT(DISTINCT(product_sku)) AS distinct_key_count
FROM products

--cte to QA distinct value count of product_sku
WITH cte AS(
	SELECT 'sales_report' AS table_name,
	COUNT(*) AS count_rows,
	'product_sku' AS key_name,
	COUNT(DISTINCT(product_sku)) AS distinct_product_sku_count
	FROM products),
	
QA_raw AS(
	SELECT COUNT(DISTINCT(distinct_product_sku_count)) AS test_result FROM cte)
	
SELECT 'referential_integrity_check' as QA_test_name,
	CASE
		WHEN test_result = 1 THEN 'Pass'
		ELSE 'Fail'
	END AS QA_result
FROM QA_raw
```

The QA_result showed Pass.

Future Goals

In general, there were a lot of risk areas from the data provided such as duplicate values, null values, lack of information about the data itself such as information about the description of columns in each table. If given more time, I would like to work more on the following:

1. Create 2 or more tables from all_sessions table to obtain more consistent data, therefore, ensuring the accuracy and reliability of the data.

2. Develop SQL queries to obtain statistical analysis for each column for each table, such as minimum, maximum, average, to help detect any potential data issues and outliers, which can indicate possible data entry errors. 

3. Create some kind of visualization to better analyze and understand the patterns and relationships between the data as a whole, which is not always obvious when just looking at numbers from tables.

