What are your risk areas? Identify and describe them.

1. In analytics table, I am assuming that the values in the revenue should be equal to the units_sold multiplied by the  unit_price, but they are not. There might be something that contributed to this discrepancy, such as a discount applied to the product, which makes it cost less, or additional fees like taxes or customs, which makes it cost more.  If given more time, I would like to dig deeper on this and find the correct formula that will makes sense of the data. 

To fix this, transform the value of revenue with the proper formula: 

SQL Query:
```
UPDATE analytics
SET revenue = (units_sold * unit_price)
WHERE revenue != (units_sold * unit_price)
```

Similar on the all_sessions table, transform the value of product_revenue with the proper formula:
```
UPDATE all_sessions
SET product_revenue = (product_quantity * product_price)
WHERE product_revenue != (product_quantity * product_price)
```

2. In products table, checking if product_sku have no duplicate values since primary key should be unique.

SQL Query:
```
SELECT product_sku, COUNT(product_sku) as pd_count
FROM products
GROUP BY product_sku
HAVING COUNT(product_sku) >1
```

The query yields no result as expected.

3. Checking referential integrity between products and sales_report table to ensure the primary key-foreign key relationship between tables remain consistent and valid using product_sku.

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

In general, there were a lot of risk areas from the data provided such as duplicate values, null values, lack of information about the data itself such as information about the description of columns in each table. If given more time, I would like to work more on the following:

1. Create 2 or more tables from all_sessions table to obtain more consistent data, therefore, ensuring the accuracy and reliability of the data.

2. Develop SQL queries to obtain statistical analysis for each column for each table, such as minimum, maximum, average, to help detect any potential data issues and outliers, which can indicate possible data entry errors. 

3. Create some kind of visualization to better analyze and understand the patterns and relationships between the data as a whole, which is not always obvious when just looking at numbers from tables.

