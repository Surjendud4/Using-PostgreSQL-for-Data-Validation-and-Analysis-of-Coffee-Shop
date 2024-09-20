# TOTAL SALES FOR EACH REPECTIVE MONTH
![image](https://github.com/user-attachments/assets/920232f5-bce4-495f-b3ba-5a981ce836a9)

code:
SELECT
	EXTRACT (YEAR FROM transaction_date) AS year,
	EXTRACT (MONTH FROM transaction_date) AS month,
	ROUND (SUM(transaction_qty * unit_price)) AS total_sales
	FROM coffee_shop_transactions
	GROUP BY
	EXTRACT (YEAR FROM transaction_date),
	EXTRACT (MONTH FROM transaction_date)
	ORDER BY year, month;
