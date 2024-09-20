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

