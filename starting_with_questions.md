Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries for city:

```SELECT city, total_transaction_revenue 
FROM all_sessions
WHERE city IS NOT null AND
    total_transaction_revenue IS NOT null
GROUP BY city, total_transaction_revenue DESC
LIMIT 10```

Answer: The top highest cities are Atlanta, Sunnyvale, Tel Aviv-Yafo, Los Angeles, and Seattle.

SQL Queries for country:

SELECT country, total_transaction_revenue 
FROM all_sessions
WHERE country IS NOT null AND
    total_transaction_revenue IS NOT null
GROUP BY country, total_transaction_revenue
ORDER BY total_transaction_revenue DESC
LIMIT 20

Answer: The top highest countries are United States, Israel, and Australia.

———————————————————————————————————————————————

Question 2: What is the average number of products ordered from visitors in each city and country?

SQL Queries:

SELECT city, country, 
	AVG(units_sold) OVER () AS avg_unit_sold,
	AVG(units_sold) OVER (PARTITION BY city) AS avg_unit_sold_by_city,
	AVG(units_sold) OVER (PARTITION by country) AS avg_unit_sold_by_country
FROM all_sessions AS als
JOIN analytics AS a
	ON als.full_visitor_id = a.full_visitor_id
WHERE city IS not null AND
	country IS NOT null

Answer: For cities, the average number of products ordered ranges from 1 to 95.  For countries, the average ranges from 1 to 52. 

———————————————————————————————————————————————

Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:

SELECT COUNT(*) AS prod_per_category, product_category, city, country
FROM all_sessions
WHERE city IS NOT null 
	AND country IS NOT null
GROUP BY product_category, city, country
ORDER BY prod_per_category, city, country
LIMIT 100

Answer: For both the city and country, the most popular category of products ordered is Home/Apparel, followed by Home/Electronics and Home/Office.

———————————————————————————————————————————————

Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

SQL Queries:

SELECT sr.total_ordered, sr.name, als.city, als.country, als.visit_id
FROM sales_report AS sr
JOIN all_sessions AS als
	ON sr.product_sku = als.product_sku
WHERE sr.total_ordered > 0
GROUP BY sr.total_ordered, sr.name, als.visit_id, als.city, als.country
ORDER BY sr.total_ordered DESC

Answer: For both the city and country, the top selling product is Ballpoint LED Light Pen, followed by 17oz Stainless Steel Sport Bottle and Leatherette Journal.

———————————————————————————————————————————————

Question 5: Can we summarize the impact of revenue generated from each city/country?

SQL Queries:

SELECT city, country, SUM(total_transaction_revenue)
FROM all_sessions
WHERE total_transaction_revenue IS NOT null AND 
	city IS NOT null AND
	country IS NOT null
GROUP BY city, country
ORDER BY SUM(total_transaction_revenue) DESC

Answer:

Most of the revenue generated is coming from the United States, followed by Israel, Australia, Canada and Switzerland. With this information, the ecommerce site can decide on focusing more on the country that generates their highest revenue, or expanding to other revenue-contributing countries - such as introducing more products and offering discounts and promotions.








