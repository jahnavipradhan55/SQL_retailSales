# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `Retail_Sales`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `Retail_Sales`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
--SQL Retail Sales Analysis- P1
Create Database ProjectPractice

--CREATE TABLE 
CREATE Table Retail_Sales 
( 
transactions_id INT Primary Key,
sale_date DATE, 
sale_time Time, 
customer_id	INt, 
gender Varchar(15),
age Int, 
category Varchar (20),
quantity Int, 
price_per_unit Float, 
cogs Float,
total_sale Float
);

```

### 3. Insert CSV File Data Into SQL 
```sql
Truncate Table Retail_Sales; 
GO 
Bulk Insert dbo.Retail_Sales 
From 'C:\Users\18727\OneDrive\Desktop\Retail-Sales-Analysis-SQL-Project--P1-main\Retail-Sales-Analysis-SQL-Project--P1-main\SQL - Retail Sales Analysis_utf .csv'
WITH 
(
	Format ='CSV', 
	FirstRow=2
)
GO
```


### 3. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql

SELECT TOP 10 * FROM Retail_Sales;


SELECT Count(*) from Retail_Sales


SELECT * FROM Retail_Sales
where
transactions_id IS NULL
or
Sale_date is null
or 
sale_time is null
or 
gender is null
or 
category is null
or
quantity is null
or
cogs is null
or
total_sale is null;


DELETE FROM Retail_Sales
where
transactions_id IS NULL
or
Sale_date is null
or 
sale_time is null
or 
gender is null
or 
category is null
or
quantity is null
or
cogs is null
or
total_sale is null;



--Data Exploration
--How many sales we have: 
SELECT COUNT(*) as TotalSales 
from Retail_Sales

--How many unique customers we have: 
SELECT COUNT(Distinct Customer_id) as TotalCustomers 
From Retail_Sales

--How many categories we have: 
SELECT distinct Category as TotalCategories 
FROM Retail_Sales

```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05'**:

```sql
SELECT * 
FROM retail_sales
where sale_date = '2022-11-05'
```

2. **Write a SQL query to retrieve all transactions where the category is 'clothing' and the quantity sold is more than 10 in the month of Nov - 2022**: 
```sql
SELECT 
    category, 
    SUM(quantity) AS TotalQuantity,
    sale_date
FROM Retail_Sales
WHERE 
    category = 'Clothing'
    AND sale_date >= '2022-11-01'
    AND sale_date < '2022-12-01'
GROUP BY category, sale_date
HAVING SUM(quantity) > 10;
```


--3. **Write a SQL query to calculate the total sales(total_sale) for each category:**
```sql
SELECT category, 
Sum(total_sale) as net_sale,
Count(*) as Total_orderss
FROM Retail_sales 
Group by Category
```

--4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category:**
```sql
SELECT 
Avg(AGE) as Average_Age, 
category
FROM Retail_Sales
where category = 'Beauty'
Group by category
```

--5. **Write a SQL query to find all transactions where the total_Sales is greater than 1000:**. 
```sql
SELECT  
*
from Retail_Sales
where total_sale > 1000
```


--6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category:**
```sql
SELECT Count(transactions_id) as Total_transactionsID,
gender, 
category
FROM Retail_Sales
Group by Gender, category
```


--7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:**
```sql
SELECT 
	Year, 
	Month,
	Avg_Sale
FROM
	(
	SELECT 
		YEAR(sale_date) AS Year,
		MONTH(sale_date) AS Month,
		AVG(total_sale) AS Avg_Sale, 
		Rank() over (partition by Year(sale_date) Order by Avg(total_sale) DESC) as RANK
	FROM Retail_Sales
	GROUP BY YEAR(sale_date), MONTH(sale_date)
) as T1
where rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total Sales:**
```sql
SELECT top 5 
customer_id, 
sum(total_sale) as Total_sale
FROM Retail_Sales
Group by customer_id
Order by Total_sale DESC
```

--9. **Write a SQL query to find the number of unique customers who purchased items from each category:**
```sql
SELECT count(distinct customer_id),
category
from Retail_Sales 
Group by category
```

--10. **Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening > 17):**
```sql
SELECT 
    CASE 
        WHEN DATEPART(HOUR, sale_time) < 12 THEN 'Morning'
        WHEN DATEPART(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS Shift,
    COUNT(*) AS Total_Orders
FROM Retail_Sales
GROUP BY 
    CASE 
        WHEN DATEPART(HOUR, sale_time) < 12 THEN 'Morning'
        WHEN DATEPART(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END;
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

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Jahnavi

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. 

