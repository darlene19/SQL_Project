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

2. In products table, checking if product_sku have no duplicate values primary key should be unique.

SQL Query:
```
SELECT product_sku, COUNT(product_sku) as pd_count
FROM products
GROUP BY product_sku
HAVING COUNT(product_sku) >1
```

The query yields no result as expected.