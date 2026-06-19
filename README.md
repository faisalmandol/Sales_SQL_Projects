# Sales_SQL_Projects
![Sales_logo](https://github.com/faisalmandol/Sales_SQL_Projects/blob/main/Sales_Retails%20.jpg)

# SQL Retail Sales Analysis 

# Schema
```sql
CREATE TABLE retail_sales (
       transactions_id INT PRIMARY KEY,
	   sale_date DATE, 
	   sale_time TIME,
	   customer_id INT,
	   gender VARCHAR(10),
	   age INT,
	   category VARCHAR(11),
	   quantity INT,
	   price_per_unit FLOAT,
	   cogs FLOAT,
	   total_sale FLOAT 
);
```
# See the DataSet
```sql
select * from retail_sales;
```
# Total_Rows
```sql
select count(*)
from retail_sales;
```sql
# Check out every column, is there any null row
# Data Cleaning 
```sql
select *
from retail_sales
where transactions_id is null;

select * 
from retail_sales
where sale_date is null;

select * 
from retail_sales
where sale_time is null;

select * 
from retail_sales
where customer_id is null;
```
# Here we can write easily
```sql
select * 
from retail_sales
where transactions_id is null
      or
	  sale_date is null
	  or
	  sale_time is null
	  or
	  customer_id is null
	  or
	  gender is null
	  or 
	  age is null
	  or 
	  category is null
	  or 
	  quantity is null
	  or 
	  price_per_unit is null
	  or 
	  cogs is null
	  or 
	  total_sale is null;
```
# If null cell exists, then delete them
```sql
delete from retail_sales
where transactions_id is null
      or
	  sale_date is null
	  or
	  sale_time is null
	  or
	  customer_id is null
	  or
	  gender is null
	  or 
	  age is null
	  or 
	  category is null
	  or 
	  quantity is null
	  or 
	  price_per_unit is null
	  or 
	  cogs is null
	  or 
	  total_sale is null;
```
# Data Exploration

# How many records we have ?
```sql
select count(*) as total_records
from retail_sales;
```
# How many unique customers we have ?
```sql
select count(distinct customer_id) as total_customers
from retail_sales;
```

# How many unique categories we have ?
```sql
select count(distinct category) as unique_categories
from retail_sales;
```
# Show the categories we have
```sql
select distinct category
from retail_sales;
```
# DATA ANALYSIS AND BUSINESS KEY PROBLEMS AND ANSWERS

 
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022
-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)


# Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'
```sql
select * 
from retail_sales
where sale_date = '2022-11-05';
```
# Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing'    and the quantity sold is more than or equal to 4 in the month of Nov-2022
```sql
select * 
from retail_sales
where category = 'Clothing'
      and 
	  quantity >= 4
	  and 
	  to_char(sale_date, 'YYYY-MM') = '2022-11'
```
# Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
```sql
select category, sum(total_sale) as total_sales
from retail_sales
group by category
order by total_sales desc;
```
# Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
```sql
select round(avg(age),2) as avg_age
from retail_sales
where category = 'Beauty';
```
# Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
```sql
select * 
from retail_sales 
where total_sale > 1000;
```
# Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
```sql
select category,
       gender,
       count(transactions_id) total_transaction
from retail_sales
group by category, gender
order by category;
```
# Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.
```sql
with cte as (

	  select extract(YEAR from sale_date) as year,
	         extract(MONTH from sale_date) as month,
			 avg(total_sale) as avg_sale,
			 rank() over(partition by extract(YEAR from sale_date) order by avg(total_sale) desc) as rnk
	  from retail_sales
	  group by year, month
)

select year,
       month,
	   avg_sale
from cte
where rnk = 1;
```
# Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
```sql
select customer_id,
        sum(total_sale) as total
from retail_sales
group by customer_id
order by total desc
limit 5;
```
# Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
```sql
select category,
       count(distinct customer_id)  as unique_id
from retail_sales
group by category;
```
# Q.10 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)
```sql
with cte as (

      select *,
	       case
	         when extract(HOUR from sale_time) < 13 then 'Morning'
			 when extract(HOUR from sale_time) between 12 and 17 then 'Afternoon'
			 else 'Evening'
		   end as shift
	  from retail_sales	  
)

select shift,
       count(*) total_orders
from cte
group by shift;
```
# End of the Project!
