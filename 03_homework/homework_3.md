# Homework 3: Essential SQL

-  	Due on Saturday, May 17 at 11:59pm
-  	Weight: 8% of total grade
-  	Upload one .sql file with your queries

# AGGREGATE
1. Write a query that determines how many times each vendor has rented a booth at the farmer’s market by counting the vendor booth assignments per `vendor_id`.

SELECT vendor_id, COUNT(booth_number)
FROM vendor_booth_assignments
GROUP BY vendor_id

2. The Farmer’s Market Customer Appreciation Committee wants to give a bumper sticker to everyone who has ever spent more than $2000 at the market. Write a query that generates a list of customers for them to give stickers to, sorted by last name, then first name. 
**HINT**: This query requires you to join two tables, use an aggregate function, and use the HAVING keyword.

SELECT  c.customer_id, c.customer_first_name, c.customer_last_name, sum(cp.quantity* cp.cost_to_customer_per_qty) AS totalpurchase 
FROM customer c
INNER JOIN  customer_purchases cp
ON c.customer_id = cp.customer_id
GROUP BY customer_last_name, customer_first_name
HAVING totalpurchase>2000
ORDER BY customer_last_name, customer_first_name





# Temp Table
1. Insert the original vendor table into a temp.new_vendor and then add a 10th vendor: Thomass Superfood Store, a Fresh Focused store, owned by Thomas Rosenthal
**HINT**: This is two total queries -- first create the table from the original, then insert the new 10th vendor. When inserting the new vendor, you need to appropriately align the columns to be inserted (there are five columns to be inserted, I've given you the details, but not the syntax)

To insert the new row use VALUES, specifying the value you want for each column:  
`VALUES(col1,col2,col3,col4,col5)`

DROP TABLE IF EXISTS temp.new_vendor;
CREATE TEMP TABLE temp.new_vendor AS
SELECT * FROM vendor;
INSERT INTO temp.new_vendor VALUES (10, 'Thomass Superfood', 'Fresh Focused', 'Rosenthal', 'Thomas');




# Date
1. Get the customer_id, month, and year (in separate columns) of every purchase in the customer_purchases table.
**HINT**: you might need to search for strfrtime modifers sqlite on the web to know what the modifers for month and year are!
SELECT customer_id, STRFTIME('%Y', market_date), STRFTIME('%m', market_date)
FROM customer_purchases

2. Using the previous query as a base, determine how much money each customer spent in April 2019. Remember that money spent is `quantity*cost_to_customer_per_qty`.
**HINTS**: you will need to AGGREGATE, GROUP BY, and filter...but remember, STRFTIME returns a STRING for your WHERE statement!!


SELECT customer_id, STRFTIME('%Y', market_date) AS market_year, STRFTIME('%m', market_date) AS market_month
, SUM(quantity * cost_to_customer_per_qty) AS totalpurchase
FROM customer_purchases
GROUP BY market_year, market_month, customer_id
HAVING market_year LIKE '2019' AND market_month LIKE '04'
