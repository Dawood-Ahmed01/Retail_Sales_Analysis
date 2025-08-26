                      📊 Retail Sales SQL Analysis
📌 Project Description

This project demonstrates SQL-based retail sales analysis on a sample dataset.
The aim is to explore sales patterns, customer behavior, and product performance through structured queries.

The queries cover:

Data extraction

Filtering & conditions

Aggregations (SUM, AVG, COUNT)

Grouping & Ordering

Window Functions (RANK, CTEs)

📂 Dataset Information

The dataset (retail_sales) contains columns such as:

transaction_id

sale_date

sale_time

category

customer_id

gender

age

quantity

total_sale

📝 SQL Questions & Queries
1. Retrieve all sales made on 2022-11-05:
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

2. Transactions in 'Clothing' with quantity > 4 (Nov-2022):
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND quantity >= 4
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';

3. Total sales for each category:
SELECT category,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY category
ORDER BY total_sales DESC;

4. Average age of customers who purchased 'Beauty' items:
SELECT category,
       ROUND(AVG(age),0) AS avg_age
FROM retail_sales
WHERE category = 'Beauty'
GROUP BY category;

5. Transactions with sales > 1000:
SELECT *
FROM retail_sales
WHERE total_sale > 1000;

6. Number of transactions by gender in each category:
SELECT category,
       gender,
       COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

7. Best selling month (average sales per month per year):
WITH mon AS (
    SELECT EXTRACT(YEAR FROM sale_date) AS year,
           EXTRACT(MONTH FROM sale_date) AS month,
           ROUND(AVG(total_sale)::NUMERIC, 2) AS avg_sale,
           RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date)
                        ORDER BY AVG(total_sale) DESC) AS ranking
    FROM retail_sales
    GROUP BY 1, 2
)
SELECT year,
       TO_CHAR(TO_DATE(month::text, 'MM'), 'Month') AS months,
       avg_sale
FROM mon
WHERE ranking = 1
ORDER BY ranking;

8. Top 5 customers based on highest sales:
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

9. Unique customers per category:
SELECT category,
       COUNT(DISTINCT customer_id) AS total_unique_custs
FROM retail_sales
GROUP BY category
ORDER BY total_unique_custs DESC;

10. Orders by time shifts (Morning, Afternoon, Evening):
WITH times AS (
    SELECT *,
           CASE 
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS time_shift
    FROM retail_sales
)
SELECT time_shift,
       COUNT(*) AS total_orders
FROM times
GROUP BY time_shift;

🚀 Key Learnings

Practical usage of WHERE, GROUP BY, ORDER BY

Applying aggregates (SUM, AVG, COUNT)

Handling dates and time shifts

Using CTEs and Window Functions for analytics

Identifying top customers & best-selling months

📎 How to Use

Clone this repo to your local machine.

Import dataset into your SQL database (PostgreSQL recommended).

Run queries from the provided SQL script.

Modify queries for deeper insights.

📜 License

This project is licensed under the MIT License – free to use and modify.

🔗 GitHub – Dawood-Ahmed01
