What are your risk areas? Identify and describe them.

In analytics table, I am assuming that the values in the revenue should be equal to the units_sold multiplied by the  unit_price, but they are not. There might be something that contributed to this discrepancy, such as a discount applied to the product, which makes it cost less, or additional fees like taxes or customs, which makes it cost more.  If given more time, I would like to dig deeper on this and find the correct formula that will makes sense of the data. 

SQL Query:

SELECT units_sold, unit_price, revenue, (units_sold * unit_price) AS actual_revenue
FROM analytics
WHERE units_sold IS NOT null AND
	unit_price IS NOT null AND
	revenue IS NOT null
