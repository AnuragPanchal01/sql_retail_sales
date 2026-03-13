# SQL Retail Sales Analysis Project

## Skills Demonstrated

- SQL Data Cleaning
- Data Exploration
- Aggregations
- Window Functions
- Common Table Expressions (CTE)
- Business Data Analysis

## Tools Used

- SQL Server
- SQL
- Data Analysis
- GitHub

## Project Overview

This project analyzes a retail sales dataset using SQL to extract business insights.
The goal of the project is to practice data cleaning, data exploration, and business analysis using SQL queries.

The dataset contains information about customer transactions such as sales date, category, quantity, price, and total sales.

## Database & Table Structure

**Database**: `sql_project_p1`  
**Table**: `retail_sales`

| Column | Description |
|------|-------------|
| transactions_id | Primary transaction ID |
| sale_date | Date of sale |
| sale_time | Time of sale |
| customer_id | Customer ID |
| gender | Gender of customer |
| age | Customer age |
| category | Product category |
| quantiy | Quantity sold |
| price_per_unit | Price per unit |
| cogs | Cost of goods sold |
| total_sale | Total sale value |

```sql
CREATE DATABASE sql_project_p1;
USE sql_project_p1;

--Creating Table
CREATE TABLE retail_sales
(
    transactions_id	int primary key,
    sale_date DATE	,
    sale_time Time,
    customer_id int ,
    gender varchar(15),
    age int ,
    category varchar(15),	
    quantiy int,
    price_per_unit DECIMAL(10,2),
    cogs DECIMAL(10,2),
    total_sale DECIMAL(10,2)
);
```

### Inserting data to retail_sales table

```sql
BULK INSERT retail_sales
FROM '/tmp/retail_sales.csv'
WITH (
    FORMAT='CSV',
    FIRSTROW = 2
);
```

### View Top 10 Rows

```sql
SELECT top 10 * from retail_sales;
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.


```sql
SELECT COUNT(*) Total_rows
FROM retail_sales;

SELECT * FROM retail_sales
where 
    transactions_id is NULL 
    or
    sale_date is NULL 
    or 
    sale_time is NULL 
    or
    customer_id is NULL
    or
    gender is NULL
    or
    age is NULL
    or
    category is NULL
    or 
    quantiy is NULL
    or
    price_per_unit is null
    or
    cogs is NULL 
    OR
    total_sale is NULL;

DELETE from retail_sales
where 
    transactions_id is NULL 
    or
    sale_date is NULL 
    or 
    sale_time is NULL 
    or
    customer_id is NULL
    or
    gender is NULL
    or
    age is NULL
    or
    category is NULL
    or 
    quantiy is NULL
    or
    price_per_unit is null
    or
    cogs is NULL 
    OR
    total_sale is NULL;

SELECT COUNT(distinct customer_id) Unique_customers
FROM retail_sales;

SELECT Distinct category
from retail_sales
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
```sql
SELECT * 
from retail_sales 
WHERE sale_date = '2022-11-05';
```
--Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022
```sql
SELECT * 
from retail_sales
where category = 'Clothing' and quantiy >3 and MONTH(sale_date)= 11 and YEAR(sale_date) = 2022;
```

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

```sql
SELECT category , SUM(total_sale) as total_sale
from retail_sales
GROUP by category;
```
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
```sql
SELECT AVG(age) as Avg_age_of_beauty_customer
from retail_sales
where category = 'Beauty';
```
-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
```sql
SELECT *
from retail_sales
where  total_sale>1000;
```
-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
```sql
SELECT category,gender,COUNT(transactions_id) as total_transactions
from retail_sales 
GROUP BY category,gender;
```
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.
```sql
SELECT Year,[Month],avg_sale from(SELECT YEAR(sale_date) Year , MONTH(sale_date) Month, AVG(total_sale) avg_sale,RANK() OVER (PARTITION by YEAR(sale_date) ORDER by AVG(total_sale) DESC) as rank
From retail_sales
GROUP BY MONTH(sale_date),YEAR(sale_date))as t1
where rank =1;
```
-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
```sql
SELECT Top 5 customer_id,SUM(total_sale) total_sales
from retail_sales
GROUP by customer_id
ORDER by total_sales DESC;
```
-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
```sql
SELECT category,COUNT(distinct customer_id) as unique_customers
from retail_sales
GROUP by category;
```
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
```sql
WITH hourly_sales
as
(
 SELECT *,
 case when  datepart(hour,sale_time) <=12 then 'Morning'
        when DATEPART(HOUR,sale_time)>17 then 'Evening'
        else 'Afternoon'
        END shift

 from retail_sales
) SELECT shift , COUNT(*) as on_of_orders 
  from hourly_sales
  GROUP by shift
```
## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.


## Author

**Anurag Panchal**

🔗 LinkedIn: https://www.linkedin.com/in/panchalanurag/  
📧 Email: anuragpanchal002@gmail.com
