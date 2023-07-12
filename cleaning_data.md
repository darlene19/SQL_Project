What issues will you address by cleaning the data?

For all_sessions table:

1. Delete columns with values that are all NULL. For the currencycode, values are all only USD.
```
ALTER TABLE all_sessions
DROP COLUMN productrefundamount,
DROP COLUMN itemquantity,
DROP COLUMN itemrevenue,
DROP COLUMN searchkeyword,
DROP COLUMN currencycode
```

2. Change the price values to a meaningful value.
```
UPDATE all_sessions
SET totaltransactionrevenue = totaltransactionrevenue / 1000000,
productprice = productprice / 1000000,
productrevenue = productrevenue / 1000000,
transactionrevenue = transactionrevenue / 1000000
```

3. Replacing missing values such as ‘(not set)’ and ‘not available in demo dataset’ to achieve consistency.
```
UPDATE all_sessions
SET country = 'null'
WHERE country = '(not set)';
```

```
UPDATE all_sessions
SET city = 'null'
WHERE city = '(not set)' OR
city = 'not available in demo dataset'
```

```
UPDATE all_sessions
SET v2productcategory = 'null'
WHERE v2productcategory = '(not set)'
```

```
UPDATE all_sessions
SET productvariant = 'null'
WHERE productvariant = '(not set)'
```

4. Changing the data types of the columns.
```
ALTER TABLE all_sessions
ALTER COLUMN fullvisitorid TYPE NUMERIC USING fullvisitorid::numeric,
ALTER COLUMN time TYPE INT USING time::integer,
ALTER COLUMN transactionid TYPE varchar USING transactionid::varchar,
ALTER COLUMN pagetitle TYPE USING page title::varchar(1000),
ALTER COLUMN totaltransactionrevenue TYPE FLOAT USING totaltransactionrevenue::float,
ALTER COLUMN productprice TYPE FLOAT USING productprice::float,
ALTER COLUMN productrevenue TYPE FLOAT USING productrevenue::float,
ALTER COLUMN transactionrevenue TYPE FLOAT USING transactionrevenue::float
```

5. Change column name for better readability and for consistency.

```
ALTER TABLE all_sessions
RENAME COLUMN pageviews TO page_views,
RENAME COLUMN sessionqualitydim TO session_quality_dim,
RENAME COLUMN visitid TO visit_id,
RENAME COLUMN productquantity TO product_quantity,
RENAME COLUMN productprice TO product_price,
RENAME COLUMN productrevenue TO product_revenue,
RENAME COLUMN productsku TO product_sku,
RENAME COLUMN v2productname TO product_name,
RENAME COLUMN v2productcategory TO product_category,
RENAME COLUMN productvariant TO product_variant,
RENAME COLUMN transactionrevenue TO transaction_revenue ,
RENAME COLUMN transactionid TO transaction_id,
RENAME COLUMN pagetitle TO page_title,
RENAME COLUMN pagepathlevel1 TO page_path_level1,
RENAME COLUMN ecommerceaction_type TO ecommerce_action_type,
RENAME COLUMN ecommerceaction_step TO ecommerce_action_step,
RENAME COLUMN ecommerceaction_option TO ecommerce_action_option
```
———————————————————————————————————————————————

For analytics table:

1. Delete columns with values that are all NULL.
```
ALTER TABLE analytics
DROP COLUMN userid 
```

2. Change the price values to a meaningful value.
```
UPDATE analytics
SET unit_price = unit_price / 1000000,
SET revenue = revenue / 1000000
```

3. Changing the data types of the columns.
```
ALTER TABLE analytics
ALTER COLUMN fullvisitorid TYPE NUMERIC USING fullvisitorid::numeric,
ALTER COLUMN revenue TYPE FLOAT USING revenue::float,
ALTER COLUMN unit_price TYPE FLOAT USING unit_price::float
```

4. Change column names for better readability and for consistency.
```
ALTER TABLE analytics
RENAME COLUMN visitnumber TO visit_number,
RENAME COLUMN visitid TO visit_id,
RENAME COLUMN visitstarttime TO visit_start_time,
RENAME COLUMN fullvisitorid TO full_visitor_id,
RENAME COLUMN channelgrouping TO channel_grouping,
RENAME COLUMN socialengagementtype TO social_engagement_type,
RENAME COLUMN pafeviews TO page_views,
RENAME COLUMN timeonsite TO time_on_site
```

5. Remove duplicate rows.
```
CREATE TABLE analytics_bckup AS
(SELECT DISTINCT * FROM analytics)

--Check if the duplicate rows were deleted.

SELECT * FROM analytics_bckup

--Truncating the original table instead of dropping it.

TRUNCATE TABLE analytics

--Populate the analytics (original table) from analytics_bckup (backup table).

INSERT INTO analytics
SELECT * FROM analytics_bckup

--Delete the backup table.

DROP TABLE analytics_bckup

--Check if the analytics table now contains unique rows.

SELECT * FROM analytics
```
———————————————————————————————————————————————

For products table:

1. Rename column sku to productsku to enable using JOIN function with other tables.
```
ALTER TABLE products
RENAME COLUMN sku TO productsku
```

2. Changing the data types of the columns.
```
ALTER TABLE products
ALTER COLUMN productsku TYPE VARCHAR USING productsku::varchar,
ALTER COLUMN name TYPE VARCHAR USING name::varchar
```

3. Changing column names for better readability and for consistency.
```
ALTER TABLE products
RENAME COLUMN productsku TO product_sku,
RENAME COLUMN orderedquantity TO ordered_quantity,
RENAME COLUMN stocklevel TO stock_level,
RENAME COLUMN restockingleadtime TO restocking_lead_time,
RENAME COLUMN sentimentscore TO sentiment_score,
RENAME COLUMN sentimentmagnitude TO sentiment_magnitude
```
———————————————————————————————————————————————

For sales_by_sku table:

1. Change the data types of the columns.
```
ALTER TABLE sales_by_sku
ALTER COLUMN productsku TYPE VARCHAR USING productsku::varchar
```

2. Assign a primary key.
```
ALTER TABLE sales_by_sku
ADD PRIMARY KEY (productsku)
```

3. Change column name for better readability and for consistency.
```
ALTER TABLE sales_by_sku
RENAME COLUMN productsku TO product_sku
```

———————————————————————————————————————————————

For sales_report table:

1. Changing the data types of the columns.
```
ALTER TABLE products
ALTER COLUMN productsku TYPE VARCHAR USING productsku::varchar,
ALTER COLUMN name TYPE VARCHAR USING name::varchar
```

2. Assign a primary key.
```
ALTER TABLE sales_report
ADD PRIMARY KEY (productsku)
```

3. Change column names for better readability and for consistency.
```
ALTER TABLE sales_report
RENAME COLUMN productsku TO product_sku,
RENAME COLUMN stocklevel TO stock_level,
RENAME COLUMN restockingleadtime TO restocking_lead_time,
RENAME COLUMN sentimentscore TO sentiment_score,
RENAME COLUMN sentimentmagnitude TO sentiment_magnitude
```
