# Walmart Data Analysis: End-to-End SQL + Python Project P-9

## Project Overview

![Project Pipeline](https://github.com/najirh/Walmart_SQL_Python/blob/main/walmart_project-piplelines.png)


This project is an end-to-end data analysis solution designed to extract critical business insights from Walmart sales data. We utilize Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques to solve key business questions. The project is ideal for data analysts looking to develop skills in data manipulation, SQL querying, and data pipeline creation.

---

## Project StepsWalmart Data Analysis: End-to-End SQL + Python Project P-9

📌 Project Overview

This project is an end-to-end data analysis solution designed to extract actionable business insights from Walmart sales data.

Python is used for data processing and cleaning.

SQL (MySQL & PostgreSQL) is used for advanced querying and business problem-solving.

Ideal for aspiring data analysts to build hands-on skills in data manipulation, SQL querying, and data pipeline creation.

🚀 Project Steps
1. Environment Setup

Tools: VS Code, Python, SQL (MySQL & PostgreSQL)

Goal: Organize project folders and create a smooth development environment.

2. Kaggle API Configuration

Get your API key from Kaggle
 → Profile → Account → API.

Save the kaggle.json file to your .kaggle/ folder.

Download datasets with:

kaggle datasets download -d <dataset-path>

3. Download Walmart Sales Data

Source: Walmart 10k Sales Dataset

Storage: Keep in data/ folder for consistency.

4. Install Required Libraries & Load Data
pip install pandas numpy sqlalchemy mysql-connector-python psycopg2


Load into Pandas for exploration.

5. Explore the Data

Use .info(), .describe(), .head() to check column types, missing values, and data distribution.

6. Data Cleaning

Remove duplicates.

Handle missing values (drop/fill).

Fix data types (e.g., convert dates, currency to floats).

Validate cleaned dataset.

7. Feature Engineering

Create Total Amount = unit_price × quantity.

Supports revenue & profitability analysis.

8. Load Data into Databases

Connect to MySQL & PostgreSQL using SQLAlchemy.

Automate table creation & insert cleaned data.

Run validation queries to ensure data integrity.

9. SQL Analysis: Business Problem Solving

Revenue trends across branches and categories.

Best-selling product categories.

Sales by city, branch, and payment method.

Peak sales hours and customer patterns.

Profit margin analysis.

🛠️ Requirements

Python: 3.8+

Databases: MySQL, PostgreSQL

Libraries: pandas, numpy, sqlalchemy, mysql-connector-python, psycopg2

Kaggle API Key

📂 Project Structure
|-- data/                     # Raw & cleaned data
|-- sql_queries/              # SQL scripts
|-- notebooks/                # Python notebooks
|-- README.md                 # Project documentation
|-- requirements.txt          # Python dependencies
|-- main.py                   # Script for cleaning & loading data

🧩 Business Problems & SQL Queries

Below are key business problems solved using SQL, with queries and their purposes:

1. Analyze Payment Methods and Sales
SELECT category, payment_method, COUNT(*) AS transactions
FROM walmart
GROUP BY category, payment_method;

2. Identify the Highest-Rated Category in Each Branch
SELECT category, branch, AVG(rating) AS avg_rating
FROM walmart
GROUP BY category, branch
ORDER BY avg_rating DESC;

3. Determine the Busiest Day for Each Branch
SELECT branch, DATEPART(WEEK, CONVERT(DATE, [date], 105)) AS week_days,
       SUM(total) AS total_sales
FROM walmart
GROUP BY branch, DATEPART(WEEK, CONVERT(DATE, [date], 105));

4. Calculate Total Quantity Sold by Payment Method
SELECT payment_method, category, COUNT(*) AS sold
FROM walmart
GROUP BY payment_method, category;

5. Analyze Category Ratings by City
SELECT category, city, AVG(rating) AS avg_rating,
       MAX(rating) AS max_rating, MIN(rating) AS min_rating
FROM walmart
GROUP BY category, city;

6. Calculate Total Profit by Category
SELECT category, SUM(profit_margin) AS total_profit
FROM walmart
GROUP BY category
ORDER BY total_profit DESC;

7. Determine the Most Common Payment Method per Branch
WITH ctc AS (
    SELECT payment_method, branch, COUNT(*) AS total,
           RANK() OVER (PARTITION BY payment_method ORDER BY COUNT(*) DESC) AS rank_1
    FROM walmart
    GROUP BY payment_method, branch
)
SELECT * FROM ctc WHERE rank_1 = 1;

8. Analyze Sales Shifts Throughout the Day
SELECT CASE 
           WHEN DATEPART(HOUR, time) < 12 THEN 'Morning'
           WHEN DATEPART(HOUR, time) BETWEEN 12 AND 18 THEN 'Afternoon'
           ELSE 'Night'
       END AS shift,
       COUNT(*) AS trans
FROM walmart
GROUP BY CASE 
             WHEN DATEPART(HOUR, time) < 12 THEN 'Morning'
             WHEN DATEPART(HOUR, time) BETWEEN 12 AND 18 THEN 'Afternoon'
             ELSE 'Night'
         END;

9. Identify Branches with Highest Revenue Decline Year-Over-Year
WITH Last_year AS (
    SELECT Branch, SUM(Total) AS Last_revenue_2019
    FROM walmart
    WHERE YEAR(TRY_CONVERT(DATE, [Date], 5)) = 2019
    GROUP BY Branch
),
This_year AS (
    SELECT Branch, SUM(Total) AS This_revenue_2020
    FROM walmart
    WHERE YEAR(TRY_CONVERT(DATE, [Date], 5)) = 2020
    GROUP BY Branch
)
SELECT l.Branch, l.Last_revenue_2019, t.This_revenue_2020,
       (t.This_revenue_2020 - l.Last_revenue_2019) AS revenue_diff,
       ROUND(((CAST(t.This_revenue_2020 AS FLOAT) - l.Last_revenue_2019)/NULLIF(l.Last_revenue_2019,0))*100,2) AS growth_percent
FROM Last_year l
JOIN This_year t ON l.Branch = t.Branch;

📊 Results & Insights

Sales Insights: Top branches, categories, and preferred payment methods.

Profitability: High-profit categories identified.

Customer Behavior: Ratings analysis by city and category.

Branch Performance: Year-over-year revenue growth/decline tracked.

🔮 Future Enhancements

Interactive dashboards (Power BI / Tableau).

Real-time data pipeline automation.

Additional datasets for deeper insights.

📜 License

This project is licensed under the MIT License.

🙏 Acknowledgments

Dataset: Kaggle – Walmart Sales Dataset

Inspiration: Walmart case studies on sales & supply chain optimization

---
