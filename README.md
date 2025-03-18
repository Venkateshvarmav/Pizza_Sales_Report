# Pizza_Sales_Report


## Project Overview

**Project Title**: Pizza Sales Report

This project is designed to demonstrate SQL and Power BI skills and techniques typically to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries and visualize the findings in power BI.

## Objectives

1.**Set up a retail sales database**: Create and populate a Pizza_Sales database with the provided dataset.
2.**Exploratory Data Analysis (EDA)**: Perform exploratory data analysis to understand the dataset.
3.**Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database setup 
* **Database Creation**: The project starts by creating a database namesd Pizza_Sales
* **Table Creation**: A table named pizza is created to store the sales dataset. The table structure includes columns for pizza_id, order_id,	pizza_name_id, quantity, order_date, order_time, unit_price, total_price, pizza_size, pizza_category, pizza_ingredients, pizza_name.


```sql
create database pizza_sales;
```
```sql
drop table if exists pizza;
```
```sql
create table pizza(
	pizza_id int,
	order_id int,
	pizza_name_id varchar(50),
	quantity int,
	order_date date,
	order_time time,
	unit_price float,
	total_price float,
	pizza_size varchar(5),
	pizza_category varchar(20),
	pizza_ingredients varchar(125),
	pizza_name varchar(50)
);
```

### 2. Data Exploration & Cleaning

* **Record Count**: Determine the total number of records in the dataset.	
* **Customer Count**: Find out how many unique orders are in the dataset.
* **Category Count**: Identify all unique pizza categories in the dataset.



```sql
select count(*) from pizza;

select count(distinct(order_id)) from pizza;

select distinct pizza_category from pizza;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Find the total revenue**

```sql
select sum(total_price) as total_revenue from pizza;
```

2. **Find the average order value**

```sql
select sum(total_price)/count(distinct(order_id)) as avg_order_value from pizza
```

3. **Find the Total Pizza Sold**

 ```sql
select sum(quantity) as total_pizza_sold from pizza
 ```

4. **find the average number of pizza ordered per order**

```sql
SELECT round((SUM(quantity)::numeric/ COUNT(DISTINCT order_id)::numeric),2)
FROM pizza;
```

5. **find the day trend of pizza ordered**

```sql
with weekname_cte as
(
  select
    to_char(order_date, 'Day') as weekname,
    count(distinct(order_id)) as number_of_orders,
    extract(DOW from order_date)
  from pizza
group by 1,3
order by extract(DOW from order_date)
)
select
  weekname,
  number_of_orders
from weekname_cte;
```

6. **find the monthly trend of pizza ordered**

```sql
with monthname_cte as
(
  select
    to_char(order_date,'Month') as month,
    extract(month from order_date),
    count(distinct(order_id)) as number_of_orders
from pizza
group by 1,2
order by 2
)
select
  month,
  number_of_orders
from monthname_cte;
```

7. **Calculate the percentage of sales by each category**

```sql   
select
  pizza_category,
  count(*) as total,
  round(sum(total_price)::numeric/(select sum(total_price) from pizza)::numeric*100,2) as pct_total_sales
from pizza
group by 1
```

 8. **Calculate the percentage of sales by pizza size**

```sql
select
  pizza_size,
  round(sum(total_price)::numeric/(select sum(total_price) from pizza)::numeric*100,2)
from pizza
group by 1
```

9. **Find the top 5 best selling pizza by total revenue**
```sql
select
  pizza_name,
  sum(total_price) as total_revenue
from pizza
group by 1
order by 2 desc
limit 5;
```

10. **Find the bottom 5 worst selling pizza by total revenue**

```sql
select
  pizza_name,
  sum(total_price) as total_revenue
from pizza
group by 1
order by 2 
limit 5
```

11. **Find the top 5 best selling pizza by total quanitity sold**
```sql
select
  pizza_name,
  count(*) as total_quanitity
from pizza  
group by 1
order by 2
limit 5
```

12. **Find the bottom 5 worst selling pizza by total quantity sold**

```sql
select
  pizza_name,
  count(*) as total_quanitity
from pizza  
group by 1
order by 2
limit 5
```

13. **Find the top 5 best selling pizza by total orders**
    
```sql
select
  pizza_name,
  count(distinct(order_id)) as total_orders from pizza
group by 1
order by 2 desc
limit 5
```

14. **Find the bottom 5 worst selling pizza by total orders**

```sql
select
  pizza_name,
  count(distinct(order_id)) as total_orders from pizza
group by 1
order by 2 
limit 5
```

15. **Find the Total revenue generated by each pizza category**

```sql
select 
	pizza_category,
	round(sum(total_price)::numeric,2) 
from pizza
group by 1
order by 2 desc
```

15. **Find the Top 5 high value orders greater than 250**

```sql
with high_order_cte as (
select 
	distinct(order_id),
	sum(total_price) as total
from pizza
group by 1
order by 2 desc
)
select * from high_order_cte
where total > 250
```


## Findings


* **High-Value Transactions**: over 35 transactions had a total sale amount greater than 250 , indicating premium purchases.
* **Sales Trends**: Daily and Monthly analysis shows variations in sales, helping identify peak seasons.
* **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

* **Sales Summary**: A detailed report summarizing total sales, category performance and pizza size performance.
* **Trend Analysis**: Insights into sales trends across different months and days of week.

## Conclusion

The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.
The same business questions have been visualised in the Power Bi report attached

## Author - Venkatesh Varma V

This project is part of my portfolio, showcasing my SQL skills essential for data analyst roles.

