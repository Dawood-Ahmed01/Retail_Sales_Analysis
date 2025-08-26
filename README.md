🛍️ Retail Sales SQL Analysis

This project analyzes a Retail Sales Dataset using PostgreSQL. The analysis covers customer behavior, sales trends, product categories, and performance insights with SQL queries.

📁 Dataset Overview

The dataset retail_sales includes:

transaction_id

customer_id

gender

age

category

quantity

price

total_sale

sale_date

sale_time

🧠 Skills Used

SQL Filtering (WHERE, BETWEEN)

Joins & Aggregations (SUM, COUNT, AVG)

Grouping (GROUP BY, HAVING)

Window Functions (RANK() OVER)

Date & Time Functions (EXTRACT, TO_CHAR)

CTEs (Common Table Expressions)

📊 Project Questions & Queries
1. Retrieve all sales made on '2022-11-05'
SELECT *  
FROM retail_sales  
WHERE sale_date = '2022-11-05';

2. Transactions where category = 'Clothing' and quantity ≥ 4 in Nov-2022
SELECT *  
FROM retail_sales  
WHERE category = 'Clothing'  
  AND quantity >= 4  
  AND TO_CHAR(sale_date , 'YYYY-MM') = '2022-11';

3. Total sales per category
SELECT category, SUM(total_sale) AS total_sales  
FROM retail_sales  
GROUP BY 1  
ORDER BY total_sales DESC;

4. Average age of customers who purchased 'Beauty' items
SELECT category, ROUND(AVG(age),0) AS avg_age  
FROM retail_sales  
WHERE category = 'Beauty'  
GROUP BY 1;

5. Transactions with total_sale > 1000
SELECT *  
FROM retail_sales  
WHERE total_sale > 1000;

6. Total transactions by gender in each category
SELECT category, gender, COUNT(*) AS total_trans  
FROM retail_sales  
GROUP BY 1, 2  
ORDER BY 1;

7. Best-selling month per year (by average sale)
WITH mon AS (  
    SELECT EXTRACT(YEAR FROM sale_date) AS year,  
           EXTRACT(MONTH FROM sale_date) AS month,  
           ROUND(AVG(total_sale)::NUMERIC ,2) AS avg_sale,  
           RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS ranking  
    FROM retail_sales  
    GROUP BY 1, 2  
)  
SELECT year, TO_CHAR(TO_DATE(month::text , 'MM') , 'Month') AS months, avg_sale  
FROM mon  
WHERE ranking = 1  
ORDER BY year;

8. Top 5 customers by total sales
SELECT customer_id, SUM(total_sale) AS total_sales  
FROM retail_sales  
GROUP BY 1  
ORDER BY total_sales DESC  
LIMIT 5;

9. Unique customers per category
SELECT category, COUNT(DISTINCT customer_id) AS total_unique_custs  
FROM retail_sales  
GROUP BY 1  
ORDER BY 2 DESC;

10. Orders by shift (Morning <12, Afternoon 12–17, Evening >17)
WITH times AS (  
    SELECT *,  
           CASE   
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'  
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'  
               ELSE 'Evening'  
           END AS time_shift  
    FROM retail_sales  
)  
SELECT time_shift, COUNT(*) AS total_orders  
FROM times  
GROUP BY 1;

📂 Files

retail_sales_analysis.sql → All SQL queries organized with comments

📜 License

This project is open-source under the MIT License
.

📬 Contact

💻 GitHub: Dawood-Ahmed01
