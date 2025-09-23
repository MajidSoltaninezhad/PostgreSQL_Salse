# Retail Sales Analysis – SQL Project

## Overview  

**Project Name**: Retail Sales Analysis  
**Difficulty**: Beginner-Friendly  
**Database**: `p1_retail_db`  

This project is designed to showcase SQL skills that every aspiring data analyst needs. From setting up a structured database to cleaning and analyzing retail transactions, the goal is to answer real-world business questions using SQL. It’s a practical starting point for anyone who wants to learn how data analysts use SQL to transform raw data into actionable insights.  

---

## Objectives  

1. Build the Database: Create and populate a retail sales database.  
2. Data Cleaning: Detect and handle missing or incomplete records.  
3. EDA (Exploratory Data Analysis): Explore the dataset to understand its structure and trends.  
4. Business Insights: Write SQL queries to answer business-related questions and discover patterns in sales.  

---

## Project Workflow  

### 1. Database Setup  

- Create the database `p1_retail_db`.  
- Build a `retail_sales` table with fields for transaction ID, sales details, customer info, and financial metrics.  

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
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
```

---

### 2. Data Cleaning & Exploration  

- Row Count – How many total records exist?  
- Unique Customers – Number of distinct buyers.  
- Categories – List of all unique product categories.  
- Missing Data Check – Identify and remove null values.  

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

DELETE FROM retail_sales
WHERE sale_date IS NULL 
   OR sale_time IS NULL 
   OR customer_id IS NULL 
   OR gender IS NULL 
   OR age IS NULL 
   OR category IS NULL 
   OR quantity IS NULL 
   OR price_per_unit IS NULL 
   OR cogs IS NULL;
```

---

### 3. Analysis & SQL Queries  

Here are the key business questions answered:  

1. Sales on a specific day (2022-11-05):  
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. Clothing orders with quantity ≥ 4 in Nov-2022:  
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

3. Total sales per category:  
```sql
SELECT category,
       SUM(total_sale) AS net_sale,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

4. Average age of Beauty customers:  
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

5. High-value transactions (> 1000):  
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

6. Transactions by gender and category:  
```sql
SELECT category,
       gender,
       COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

7. Best-selling month each year:  
```sql
SELECT year, month, avg_sale
FROM (
    SELECT EXTRACT(YEAR FROM sale_date) AS year,
           EXTRACT(MONTH FROM sale_date) AS month,
           AVG(total_sale) AS avg_sale,
           RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) t
WHERE rank = 1;
```

8. Top 5 customers by sales:  
```sql
SELECT customer_id,
       SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

9. Unique buyers per category:  
```sql
SELECT category,
       COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

10. Orders by time of day (Morning, Afternoon, Evening):  
```sql
WITH sales_by_shift AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM sales_by_shift
GROUP BY shift;
```

---

## Key Insights  

- Demographics: Customers span different age groups, showing diversity in buyers.  
- Premium Sales: Several orders crossed the 1000 mark, indicating high-value customers.  
- Seasonality: Certain months outperform others, helping identify sales peaks.  
- Top Buyers: A small group of customers drives significant revenue.  
- Category Trends: Clothing and Beauty remain strong contributors across shifts.  

---

## Reports  

- Sales Overview: Revenue, transactions, and customer breakdown.  
- Trend Analysis: Monthly and time-shift patterns.  
- Customer Insights: Spending behavior and top buyers.  

---

## Conclusion  

This project provides a solid foundation in SQL for data analysis. From setting up a database to extracting insights, the queries demonstrate how SQL supports business decision-making by uncovering sales patterns, customer behavior, and performance metrics.  

---

## How to Run  

1. Clone Repository: Download or clone this project.  
2. Set Up Database: Run `database_setup.sql` to create tables and load data.  
3. Run Queries: Use `analysis_queries.sql` for analysis.  
4. Experiment: Modify queries to explore additional insights.  

---

## Author – NEXAD  

This project is part of my learning and portfolio as a data analyst.  
Feel free to connect with me and share feedback!  



- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/majid-soltani-nezhad-1529ab247/)

