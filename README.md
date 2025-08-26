**🛍️ Retail Sales – SQL Data Analysis Project**

This project performs SQL-based analysis on retail sales data to uncover key insights about customer behavior, product categories, and sales trends.

📁 Project Overview

Dataset: retail_sales

Tools: PostgreSQL

Objective: Answer business questions using SQL queries (filtering, grouping, ranking, CTEs, and window functions).

🧠 Skills Used

Filtering & Conditional Queries (WHERE, CASE, BETWEEN)

Aggregations (SUM, COUNT, AVG)

Grouping & Ordering (GROUP BY, ORDER BY)

Window Functions (RANK(), OVER)

CTEs (WITH Clause)

Date & Time Functions (EXTRACT, TO_CHAR)

🔎 Queries & Insights

Sales on a specific date (2022-11-05).

Clothing transactions with quantity > 4 in Nov-2022.

Total sales by category (ranked).

Average customer age for Beauty category.

High-value transactions (>1000).

Transactions by gender in each category.

Best selling month in each year (using window functions).

Top 5 customers by total sales.

Unique customers per category.

Sales by time shifts (Morning, Afternoon, Evening).

📊 Example Query (Best Selling Month Each Year)
with mon as (
    select
        extract(Year from sale_date) as year,
        extract(Month from sale_date) as month,
        round(avg(total_sale)::numeric ,2) as avg_sale,
        rank() over (
            partition by extract(Year from sale_date)
            order by avg(total_sale) desc
        ) as ranking
    from retail_sales
    group by 1 , 2
)
select 
    year,
    to_char(to_date(month::text , 'MM') , 'Month') as months,
    avg_sale
from mon 
where ranking = 1
order by ranking;

📂 Files

retail_sales_analysis.sql → Contains all 10 queries.

📜 License

Open-source under the MIT License.

📬 Contact

🔗 GitHub – Dawood-Ahmed01
