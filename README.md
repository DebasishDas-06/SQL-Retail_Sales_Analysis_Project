
# Project Title
# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Intermediate
**Database**: `retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retail_db`.
- **Table Creation**: A table named `sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
create database retail_db;
use retail_db;
create table sales(
transaction_id int primary key,
sale_date date,
sale_time time,
customer_id smallint,
gender char(20),
age int,
category varchar(50),
quantity smallint,
price_per_unit float,
cogs float,
total_sales float);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
-- count total values in dataset
select count(*) from sales;

-- find out how many unique customers are in the dataset
select distinct(count(*)) from sales;

-- identify all unique products in the categorey dataset
select count(distinct(category)) from sales;

-- null check values
select *from sales
where transaction_id is null or sale_date is null or sale_time is null or category is null or gender is null or  age is null
or quantity is null  or price_per_unit is null or  cogs is null or total_sales is null;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
select*from sales where sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
select*from sales
where category = 'clothing' AND quantity > 4 and  monthname(sale_date) = 'NOVEMBER' ;

```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT*FROM SALES;
SELECT category,SUM(TOTAL_SALES) as total
from sales
group by category
order by(sum(total_sales)) desc;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
select category , avg(age) from sales
where category = 'beauty';

```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM sales
WHERE total_sales > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select gender,category,count(transaction_id)
from sales
group by gender, category;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
with ranked_sales
as(
SELECT YEAR(sale_date) as year, monthname(sale_date) as month ,sum(total_sales) as money,
row_number() over(partition by year(sale_date)  order by sum(total_sales)desc ) as rk
from sales
group by year(sale_date), monthname (sale_date))

select year,month,money 
from ranked_sales
where rk = 1;

```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
select customer_id,sum(total_sales), 
rank() over(partition by customer_id) as highest_rank from sales
     group by customer_id 
     order by sum(total_sales) desc limit 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
select count(distinct(customer_id)) from sales
group by category;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM sales
)
SELECT 
    year(sale_date),monthname(sale_date),shift,
    SUM(quantity) as total_orders    
FROM hourly_sale
GROUP BY year(sale_date), monthname(sale_date),shift;

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






