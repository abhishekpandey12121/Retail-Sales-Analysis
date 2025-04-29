📊 Retail Sales Analysis SQL Project
🔍 Overview
Project Title: Retail Sales Analysis
Dataset: SQL - Retail Sales Analysis_utf.csv
Database: p1_retail_db

This project demonstrates SQL skills used by data analysts to analyze retail sales data. It involves creating a database, cleaning the data, and using SQL queries to extract key business insights.

🎯 Objectives
Database Setup – Create a retail database and import CSV data.

Data Cleaning – Handle missing/null values for accurate analysis.

EDA & Business Insights – Use SQL to explore and answer key business questions like sales trends, top categories, and customer demographics.

📁 Project Structure
1. Database Setup
   CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
 Import your data from SQL - Retail Sales Analysis_utf.csv into the retail_sales table.

2. Data Cleaning & Exploration
   -- Record count
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Null check
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Remove invalid entries
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
    
3. Data Analysis Queries
🗓️ Sales on a Specific Day
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';

👕 High-Quantity Clothing Orders in Nov 2022
SELECT * FROM retail_sales
WHERE category = 'Clothing' AND quantity > 4
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';

💰 Total Sales by Category
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

👨‍👩‍👧‍👦 Avg Age of Beauty Category Customers
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

💸 Transactions Over ₹1000
SELECT * FROM retail_sales
WHERE total_sale > 1000;

👨‍🦰 Gender-wise Sales per Category
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

📅 Best Selling Month by Year
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS t1
WHERE rank = 1;

🥇 Top 5 Customers by Sales
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

📦 Unique Customers per Category
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

🕒 Orders by Shift (Morning, Afternoon, Evening)
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;

📌 Insights
High Sales in specific months and time slots (e.g., Afternoon).

Top Performers: Categories like Clothing and customers with the highest total purchases.

Demographics: Age and gender-wise segmentation available for deeper customer targeting.

📄 Files Included
SQL - Retail Sales Analysis_utf.csv – Dataset (Retail Sales)

sql_query_p1.sql – SQL script with all above queries

✅ How to Run
Clone the repo

Import the SQL script and CSV into your SQL tool (PostgreSQL/MySQL/SQLite)

Run queries as needed or expand for further business insights

