Question 1: What channel grouping drives the highest revenue in each city and country?

SQL Queries:

SELECT als.country, als.city, als.channel_grouping, a.revenue 
	 FROM all_sessions AS als 
JOIN analytics AS a 
	ON als.visit_id = a.visit_id
	WHERE country IS NOT NULL AND
	 	city IS NOT null AND
	 	revenue IS NOT null
GROUP BY als.country, als.channel_grouping, a.revenue, als.city 
ORDER BY a.revenue DESC

Answer: For both city and country, referral drives the most revenue, followed by Organic Search and Display.

———————————————————————————————————————————————


Question 2: What is the top 3 most expensive product sold?

SQL Queries:

SELECT als.product_name, a.unit_price 
FROM analytics AS a
JOIN all_sessions AS als
	ON a.visit_id = als.visit_id
GROUP BY als.product_name, a.unit_price 
ORDER BY a.unit_price DESC 
Limit 5

Answer: 

1. Nest Learning Thermostat 3rd Gen-USA - White  $395
2. SPF-15 Slim & Slender Lip Balm $316
3. Nest Learning Thermostat 3rd Gen-USA - Stainless Steel $298
4. 22 oz YouTube Bottle Infuser $250
5. Android Youth Short Sleeve T-shit Aqua

———————————————————————————————————————————————

Question 3: What are the top 5 most viewed product categories?

SQL Queries:

SELECT product_category, COUNT(*) AS view_count
FROM all_sessions 
WHERE country IS NOT null AND
	city IS NOT null AND 
	product_category IS NOT null
GROUP BY product_category
ORDER BY view_count DESC
LIMIT 5

Answer:

1. Home/Shop by Brand/YouTube/     2448 views
2. Home/Apparel/Men’s/Men’s-T-Shirts/  1955 views
3. Home Electronics/   852 views
4. Home/Apparel/    831 views
5. Home/Shop by Brand/Google   729 views


