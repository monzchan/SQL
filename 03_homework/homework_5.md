# Homework 5: Expanding your Database

-  	Due on Friday, May 24 at 11:59pm
-  	Weight: 8% of total grade
-  	Upload one .sql file with your queries

# Cross Join
1. Suppose every vendor in the `vendor_inventory` table had 5 of each of their products to sell to **every** customer on record. How much money would each vendor make per product? Show this by vendor_name and product name, rather than using the IDs.

**HINT**: Be sure you select only relevant columns and rows. Remember, CROSS JOIN will explode your table rows, so CROSS JOIN should likely be a subquery. Think a bit about the row counts: how many distinct vendors, product names are there (x)? How many customers are there (y). Before your final group by you should have the product of those two queries (x\*y). 

SELECT DISTINCT vendor_name, product_name, cost_to_customer_per_qty,
cost_to_customer_per_qty *5 *28 AS total_sale
FROM vendor v
JOIN vendor_inventory vi
ON v. vendor_id = vi.vendor_id
JOIN product p
ON vi.product_id = p.product_id
JOIN customer_purchases cp
ON p.product_id = cp.product_id

# INSERT
1. Create a new table "product_units". This table will contain only products where the `product_qty_type = 'unit'`. It should use all of the columns from the product table, as well as a new column for the `CURRENT_TIMESTAMP`.  Name the timestamp column `snapshot_timestamp`.

DROP TABLE IF EXISTS temp.product_units;
CREATE TEMP TABLE product_units AS
SELECT *, CURRENT_TIMESTAMP AS snapshot_timestamp 
FROM product
WHERE product_qty_type = 'unit'




2. Using `INSERT`, add a new row to the table (with an updated timestamp). This can be any product you desire (e.g. add another record for Apple Pie). 

INSERT INTO product_units
VALUES(50,'Apple pie', '300lb', 3,'unit', CURRENT_TIMESTAMP)


# DELETE 
1. Delete the older record for the whatever product you added.

**HINT**: If you don't specify a WHERE clause, [you are going to have a bad time](https://imgflip.com/i/8iq872).

DELETE FROM product_units
WHERE product_id=50

# UPDATE
1. We want to add the current_quantity to the product_units table. First, add a new column, `current_quantity` to the table using the following syntax.
```
ALTER TABLE product_units
ADD current_quantity INT;
```

Then, using `UPDATE`, change the current_quantity equal to the **last** `quantity` value from the vendor_inventory details. 

**HINT**: This one is pretty hard. First, determine how to get the "last" quantity per product. Second, coalesce null values to 0 (if you don't have null values, figure out how to rearrange your query so you do.) Third, `SET current_quantity = (...your select statement...)`, remembering that WHERE can only accommodate one column. Finally, make sure you have a WHERE statement to update the right row, you'll need to use `product_units.product_id` to refer to the correct row within the product_units table. When you have all of these components, you can run the update statement.

ALTER TABLE product_units
ADD current_quantity INT;

-----------------------------

DROP TABLE IF EXISTS temp.product_quantity;
CREATE TEMP TABLE product_quantity AS
SELECT product_id AS product_id,
COALESCE(quantity, 0)  AS quantity
FROM
(SELECT p.product_id, m.quantity
FROM product AS p
LEFT JOIN 
(SELECT x.product_id, x.quantity
FROM (
SELECT *,
RANK() OVER(PARTITION BY product_id ORDER BY market_date DESC) as current_transaction
FROM  vendor_inventory)x
WHERE x.current_transaction=1) as m
ON p.product_id = m.product_id)


UPDATE product_units
SET  current_quantity = (
SELECT quantity
FROM product_quantity
WHERE product_units.product_id = product_quantity.product_id
)