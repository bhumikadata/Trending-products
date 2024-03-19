# Trending-products

## Overview
This SQL query is designed to identify trending products for each month based on sales data. Trending products are those for which the sales in any given month are greater than the sum of the sales in the previous two months. The output starts from the 3rd month onwards to exclude the initial period where this metric may not be meaningful.

### Table Structure
This query assumes the existence of a table named orders with the following columns:

order_month (date): Date of the order,

product_id (varchar(5)): ID of the product,

sales (int): Number of sales for the product in the given month

![day18](https://github.com/bhumikadata/Trending-products/assets/131578649/0c37aabc-3edb-4e0b-ab6e-500a75294c5c)


#### SQL Query
```sql
WITH cte as
 (
    SELECT
        *,
        SUM(sales) OVER (PARTITION BY product_id ORDER BY order_month ROWS BETWEEN 2 PRECEDING AND 1 PRECEDING) as lastmonth_sales,
        ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY order_month) as rn
    FROM
        orders
)
SELECT
    order_month,
    product_id 
FROM 
    cte 
WHERE 
    rn >= 3 AND sales > lastmonth_sales;
