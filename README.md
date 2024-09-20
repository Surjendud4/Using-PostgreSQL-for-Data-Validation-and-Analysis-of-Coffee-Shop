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

