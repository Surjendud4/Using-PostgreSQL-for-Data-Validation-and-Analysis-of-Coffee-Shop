# TOTAL SALES FOR EACH REPECTIVE MONTH

SELECT

EXTRACT (YEAR FROM transaction_date) AS year,
 
 EXTRACT (MONTH FROM transaction_date) AS month,
 
 ROUND (SUM(transaction_qty * unit_price)) AS total_sales
 
 FROM coffee_shop_transactions
 
 GROUP BY
 
 EXTRACT (YEAR FROM transaction_date),
 
 EXTRACT (MONTH FROM transaction_date)
 
 ORDER BY year, month;
 
![image](https://github.com/user-attachments/assets/920232f5-bce4-495f-b3ba-5a981ce836a9)

SELECT
	
 SUM (transaction_qty * unit_price) AS total_sales
 
 FROM coffee_shop_transactions
 
![image](https://github.com/user-attachments/assets/cb94d2d9-067c-4d7a-a15a-0ded4aaaf629)

SELECT
	SUM (transaction_qty * unit_price) AS total_sales
	FROM coffee_shop_transactions
	WHERE
	EXTRACT (MONTH FROM transaction_date) = 5; -- may month

![image](https://github.com/user-attachments/assets/055dd7bf-36de-46d0-afa4-17066bdec8f5)

# Current Determine the MoM Increase or Decrease  in Sales

WITH monthly_sales AS
(

SELECT

EXTRACT (YEAR FROM transaction_date) AS year,

EXTRACT (MONTH FROM transaction_date) AS month,

ROUND (SUM (transaction_qty * unit_price)) AS total_sales

FROM coffee_shop_transactions

GROUP BY

EXTRACT (YEAR FROM transaction_date),

EXTRACT (MONTH FROM transaction_date)
)

SELECT

year,

month,

total_sales,

LAG(total_sales) OVER (ORDER  BY year,month) AS previous_month_sales,

(total_sales - LAG(total_sales) OVER ( ORDER BY year, month)) AS sales_difference,

ROUND((total_sales - LAG(total_sales) OVER (ORDER BY year, month)) /

NULLIF(LAG(total_sales) OVER (ORDER BY year, month), 0) * 100, 2) AS mom_percentage_change

FROM 

monthly_sales

ORDER BY 

year, month;

![image](https://github.com/user-attachments/assets/e3e22a15-7499-47f7-aca2-1dd46852166a)

Explanation:
1.	WITH monthly_sales AS (...): This common table expression (CTE) calculates the total sales for each month.
2.	EXTRACT(YEAR FROM transaction_date) AS year, EXTRACT(MONTH FROM transaction_date) AS month: Extracts the year and month from the transaction date.
3.	SUM(transaction_qty * unit_price) AS total_sales: Sums the total sales for each month.
4.	LAG(total_sales) OVER (ORDER BY year, month) AS previous_month_sales: The LAG() function gets the total sales of the previous month.
5.	(total_sales - LAG(total_sales) OVER (ORDER BY year, month)) AS sales_difference: Calculates the difference in sales between the current month and the previous month.
6.	ROUND((total_sales - LAG(total_sales) OVER (ORDER BY year, month)) / NULLIF(LAG(total_sales) OVER (ORDER BY year, month), 0) * 100, 2) AS mom_percentage_change: Calculates the percentage change in sales from the previous month and rounds it to two decimal places. The NULLIF() function prevents division by zero.


# Current Month vs Previous Month

SELECT 

EXTRACT(MONTH FROM transaction_date) AS month,

ROUND(SUM(unit_price * transaction_qty)) AS total_sales,

ROUND((SUM(unit_price * transaction_qty) - LAG(SUM(unit_price * transaction_qty), 1) 

OVER (ORDER BY EXTRACT(MONTH FROM transaction_date))) / 

LAG(SUM(unit_price * transaction_qty), 1) 

OVER (ORDER BY EXTRACT(MONTH FROM transaction_date)) * 100, 2
    )

AS mom_increase_percentage

FROM 

coffee_shop_transactions

WHERE

EXTRACT(MONTH FROM transaction_date) IN (4, 5)

AND EXTRACT(YEAR FROM transaction_date) = 2023

GROUP BY 

EXTRACT(MONTH FROM transaction_date)

ORDER BY 

EXTRACT(MONTH FROM transaction_date);

![image](https://github.com/user-attachments/assets/bb065cbc-5e96-4319-aaba-65e627fedfb8)

Explanation:

1.	EXTRACT(MONTH FROM transaction_date) AS month: Extracts the month from the transaction date.
2.	ROUND(SUM(unit_price * transaction_qty)) AS total_sales: Calculates the total sales for each month and rounds it to the nearest whole number.
3.	ROUND( (SUM(unit_price * transaction_qty) - LAG(SUM(unit_price * transaction_qty), 1) OVER (ORDER BY EXTRACT(MONTH FROM transaction_date))) / LAG(SUM(unit_price * transaction_qty), 1) OVER (ORDER BY EXTRACT(MONTH FROM transaction_date)) * 100, 2 ) AS mom_increase_percentage:
i)	Uses the LAG function to get the total sales of the previous month.
ii)	Calculates the month-over-month percentage change and rounds it to two decimal places.
4.	WHERE EXTRACT(MONTH FROM transaction_date) IN (4, 5) AND EXTRACT(YEAR FROM transaction_date) = 2023: Filters the data to include only transactions from April and May 2023.
5.	GROUP BY EXTRACT(MONTH FROM transaction_date): Groups the data by month.
6.	ORDER BY EXTRACT(MONTH FROM transaction_date): Orders the results by month.

# TOTAL ORDERS FOR EACH REPECTIVE MONTH

SELECT

EXTRACT (YEAR FROM transaction_date) AS year,

EXTRACT (MONTH FROM transaction_date) AS month,

COUNT(transaction_id) AS total_orders

FROM coffee_shop_transactions

GROUP BY

EXTRACT (YEAR FROM transaction_date),

EXTRACT (MONTH FROM transaction_date)

ORDER BY year, month;

![image](https://github.com/user-attachments/assets/c1a79aff-8902-4d8b-8410-b50e2bd9b55e)

SELECT COUNT

(transaction_id) AS total_orders FROM coffee_shop_transactions;

![image](https://github.com/user-attachments/assets/882feea7-cdf2-46b1-b0f6-b90bed149431)

SELECT COUNT (transaction_id) AS total_orders FROM coffee_shop_transactions

WHERE EXTRACT ( MONTH FROM transaction_date) = 5; -- may month

![image](https://github.com/user-attachments/assets/65bbf614-e976-4d43-8f84-c62c58085b37)

# TOTAL ORDERS KPI - MOM DIFFERENCE AND MOM GROWTH

WITH monthly_orders AS (
    
SELECT

EXTRACT(YEAR FROM transaction_date) AS year,

EXTRACT(MONTH FROM transaction_date) AS month,

COUNT(transaction_id) AS total_orders

FROM 

coffee_shop_transactions

GROUP BY

EXTRACT(YEAR FROM transaction_date),

EXTRACT(MONTH FROM transaction_date)
)

SELECT

year,

month,

total_orders,

LAG(total_orders) OVER (ORDER BY year, month) AS previous_month_orders,

(total_orders - LAG(total_orders) OVER (ORDER BY year, month)) AS order_difference,

ROUND(

(total_orders - LAG(total_orders) OVER (ORDER BY year, month))::numeric /

NULLIF(LAG(total_orders) OVER (ORDER BY year, month), 0) * 100, 2
    )

AS mom_percentage_change

FROM 

monthly_orders

ORDER BY 

year, month;
![image](https://github.com/user-attachments/assets/b9d96b38-ff00-4643-84f0-4f4031aa81e2)

# Current Month vs Previous Month

SELECT 

EXTRACT(MONTH FROM transaction_date) AS month,

ROUND(COUNT(transaction_id)) AS total_orders,

ROUND((COUNT(transaction_id) - LAG(COUNT(transaction_id), 1) 

OVER (ORDER BY EXTRACT(MONTH FROM transaction_date)))::numeric / 

LAG(COUNT(transaction_id), 1) 

OVER (ORDER BY EXTRACT(MONTH FROM transaction_date)) * 100, 2
    )
    
AS mom_increase_percentage

FROM 

coffee_shop_transactions

WHERE

EXTRACT(MONTH FROM transaction_date) IN (4, 5)

AND EXTRACT(YEAR FROM transaction_date) = 2023

GROUP BY 

EXTRACT(MONTH FROM transaction_date)

ORDER BY 

EXTRACT(MONTH FROM transaction_date);

![image](https://github.com/user-attachments/assets/a0168dbc-3411-47be-9755-96c9e1b4b5e5)

WITH monthly_orders AS (

SELECT

EXTRACT(YEAR FROM transaction_date) AS year,

EXTRACT(MONTH FROM transaction_date) AS month,

COUNT(transaction_id) AS total_orders

FROM 

coffee_shop_transactions

WHERE

EXTRACT(YEAR FROM transaction_date) = 2023

AND EXTRACT(MONTH FROM transaction_date) IN (4, 5)

GROUP BY

EXTRACT(YEAR FROM transaction_date),

EXTRACT(MONTH FROM transaction_date)
)

SELECT

year,

month,

total_orders,

LAG(total_orders) OVER (ORDER BY year, month) AS previous_month_orders,

(total_orders - LAG(total_orders) OVER (ORDER BY year, month)) AS order_difference,

ROUND(

((total_orders - LAG(total_orders) OVER (ORDER BY year, month))::numeric / 

NULLIF(LAG(total_orders) OVER (ORDER BY year, month), 0)) * 100, 4
    )

AS mom_increase_percentage

FROM 

monthly_orders

ORDER BY 

year, month;

![image](https://github.com/user-attachments/assets/f6cb9737-72ab-4a4f-b370-bb42c646574e)

# TOTAL QUANTITY SOLD

SELECT

EXTRACT (YEAR FROM transaction_date) AS year,

EXTRACT (MONTH FROM transaction_date) AS month,

SUM (transaction_qty) AS total_qty_sold

FROM coffee_shop_transactions

GROUP BY

EXTRACT (YEAR FROM transaction_date),

EXTRACT (MONTH FROM transaction_date)

ORDER BY year, month;

![image](https://github.com/user-attachments/assets/5ae94120-dad7-4993-8e11-e48bd6de3670)

# TOTAL QUANTITY SOLD KPI - MOM DIFFERENCE AND MOM GROWTH

WITH monthly_qty_sold AS (

SELECT

EXTRACT(YEAR FROM transaction_date) AS year,

EXTRACT(MONTH FROM transaction_date) AS month,

ROUND(SUM(transaction_qty)) AS total_qty_sold

FROM coffee_shop_transactions

GROUP BY

EXTRACT(YEAR FROM transaction_date),

EXTRACT(MONTH FROM transaction_date)
)

SELECT

year,

month,

total_qty_sold,

LAG(total_qty_sold) OVER (ORDER BY year, month) AS previous_month_qty,

(total_qty_sold - LAG(total_qty_sold) OVER (ORDER BY year, month)) AS qty_difference,

ROUND(

((total_qty_sold::numeric - LAG(total_qty_sold) OVER (ORDER BY year, month)::numeric) /

NULLIF(LAG(total_qty_sold) OVER (ORDER BY year, month), 0)::numeric) * 100, 2
    )

AS mom_percentage_change

FROM 

monthly_qty_sold

ORDER BY 

year, month;

![image](https://github.com/user-attachments/assets/e52a0c7b-e283-416a-b5dd-cd993756e9b7)

# Current Month vs Previous Month

WITH monthly_qty_sold AS (

SELECT

EXTRACT(MONTH FROM transaction_date) AS month,

ROUND(SUM(transaction_qty)) AS total_qty_sold

FROM 

coffee_shop_transactions

WHERE

EXTRACT(MONTH FROM transaction_date) IN (4, 5)

GROUP BY

EXTRACT(MONTH FROM transaction_date)
)

SELECT

month,

total_qty_sold,

ROUND(

((total_qty_sold::numeric - LAG(total_qty_sold) OVER (ORDER BY month)::numeric) /

NULLIF(LAG(total_qty_sold) OVER (ORDER BY month), 0)::numeric) * 100, 2
    )

AS mom_percentage_change

FROM 

monthly_qty_sold

ORDER BY 

month;


![image](https://github.com/user-attachments/assets/14b69b60-920e-496b-9278-e628273e9b46)

WITH monthly_qty_sold AS (

SELECT

EXTRACT(YEAR FROM transaction_date) AS year,

EXTRACT(MONTH FROM transaction_date) AS month,

ROUND(SUM(transaction_qty)) AS total_qty_sold

FROM 

coffee_shop_transactions

WHERE

EXTRACT(YEAR FROM transaction_date) = 2023

AND EXTRACT(MONTH FROM transaction_date) IN (4, 5)

GROUP BY

EXTRACT(YEAR FROM transaction_date),

EXTRACT(MONTH FROM transaction_date)
)

SELECT

year,

month,

total_qty_sold,

LAG(total_qty_sold) OVER (ORDER BY year, month) AS previous_month_qty,

(total_qty_sold - LAG(total_qty_sold) OVER (ORDER BY year, month)) AS qty_difference,

ROUND(

((total_qty_sold::numeric - LAG(total_qty_sold) OVER (ORDER BY year, month)::numeric) /

NULLIF(LAG(total_qty_sold) OVER (ORDER BY year, month), 0)::numeric) * 100, 2
    )
    
AS mom_percentage_change

FROM 

monthly_qty_sold

ORDER BY 

year, month;

![image](https://github.com/user-attachments/assets/a7c4a465-596a-466f-a06d-3fc11f8b7fba)

# CALENDAR TABLE – DAILY SALES, QUANTITY and TOTAL ORDERS

SELECT

EXTRACT (YEAR FROM transaction_date) AS year,

EXTRACT (MONTH FROM transaction_date) AS month,

EXTRACT (DAY FROM transaction_date) AS day,

SUM (unit_price * transaction_qty) AS total_Sale,

COUNT (transaction_qty) AS total_orders,

SUM (transaction_qty) AS total_qunatity

FROM

coffee_shop_transactions

WHERE

EXTRACT (YEAR FROM transaction_date) = 2023

GROUP BY

EXTRACT(YEAR FROM transaction_date),

EXTRACT(MONTH FROM transaction_date),

EXTRACT(DAY FROM transaction_date)

ORDER BY

year, month, day;

![image](https://github.com/user-attachments/assets/d09eb866-65cc-4bb2-9c62-5c63b9c192e3)

# SALES TREND OVER PERIOD

WITH sales_data AS (

SELECT

transaction_date,

EXTRACT(DOW FROM transaction_date) AS day_of_week,

CASE 
        
WHEN EXTRACT(DOW FROM transaction_date) IN (0, 6) THEN 'Weekend'

ELSE 'Weekday'

END AS day_type,

SUM(unit_price * transaction_qty) AS total_sales,

COUNT(transaction_id) AS total_orders,

SUM(transaction_qty) AS total_quantity

FROM 

coffee_shop_transactions

GROUP BY

transaction_date
)

SELECT

day_type,

SUM(total_sales) AS total_sales,

SUM(total_orders) AS total_orders,

SUM(total_quantity) AS total_quantity

FROM

sales_data

GROUP BY

day_type

ORDER BY

day_type;


![image](https://github.com/user-attachments/assets/c7173021-bafc-4f85-99c8-613c30142ae4)



