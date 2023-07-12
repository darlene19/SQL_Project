Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries for city:

SELECT city, total_transaction_revenue 
FROM all_sessions
WHERE city IS NOT null AND
    total_transaction_revenue IS NOT null
GROUP BY city, total_transaction_revenue DESC
LIMIT 10

Answer: The top highest cities are Atlanta, Sunnyvale, Tel Aviv-Yafo, Los Angeles, and Seattle.

SQL Queries for country:

SELECT country, total_transaction_revenue 
FROM all_sessions
WHERE country IS NOT null AND
    total_transaction_revenue IS NOT null
GROUP BY country, total_transaction_revenue
ORDER BY total_transaction_revenue DESC
LIMIT 20


Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







