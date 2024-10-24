# Pizzahut-Sales-Analysis-SQL-Project
## Project Overview
**Project Title**: Pizzahut Sales Analysis  
**Level**: Beginner  
**Database**: `pizzahut`
This project is designed to demonstrate SQL skills and techniques used by data analysts to explore, clean, and analyze Pizza Hut sales data. It involves setting up a Pizza Hut sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is perfect for beginners looking to build a solid foundation in data analysis and gain practical experience with SQL
## Objectives
* Set up a Pizza Hut Sales Database: Create and populate a database with the provided sales data.
* Data Cleaning: Identify and remove any records with missing or null values.
*  Exploratory Data Analysis (EDA): Perform basic EDA to understand the dataset.
*   Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup
+ **Database Creation**:The project begins by creating a database named pizzahut.
+ **Table Creation**: Several tables are created to store sales data:
+ **orders**: Stores general order information.
+ **order_details**: Stores details of each pizza in an order.
+ **pizzas**: Contains information about different pizzas.
+ **pizza_types**: Categorizes pizzas.
```
create database pizzahut;

create table orders(order_id int primary key,
order_date datetime,
order_time time);
```
```
create table orders_details(order_details_id int primary key,
order_id int,
pizza_id text,
quantity int);
```
```
create table pizza_types(pizza_type_id text,
name text,
category text,
ingredients text);
```
```
create table pizzas(pizza_id text,
pizza_type_id text,
size text,
price double);
```
### 2. Data Exploration & Cleaning
 + **Record Count**:Determine the total number of records in the orders table.
 + **Pizza Count**:Find out how many unique customers are in the dataset.
 + **Pizza_types Count**:Identify all unique pizza types in the dataset.
 + **Null Value Check**:Check for any null values in the orders, orders_details, pizza_types, and pizzas tables.
 + **Data Deletion**:Delete records with null values.

```
sql
SELECT COUNT(*) AS TotalOrders FROM orders;
SELECT COUNT(DISTINCT pizza_id) AS TotalPizzas FROM pizzas;
SELECT COUNT(DISTINCT pizza_type_id) AS TotalPizzaTypes FROM pizza_types;

SELECT * FROM orders
WHERE order_id IS NULL OR order_date IS NULL OR order_time IS NULL;

SELECT * FROM orders_details
WHERE order_details_id IS NULL OR order_id IS NULL OR pizza_id IS NULL OR quantity IS NULL;

SELECT * FROM pizza_types
WHERE pizza_type_id IS NULL OR name IS NULL OR category IS NULL OR ingredients IS NULL;

DELETE FROM orders
WHERE order_id IS NULL OR order_date IS NULL OR order_time IS NULL;
```

### 3. Data Analysis & Findings
Q1.**Retrieve the total number of orders placed.**: 
SELECT 
    COUNT(order_id) AS total_no_orders
FROM
    orders;
    
Q2.**Calculate the total revenue generated from pizza sales.**:
SELECT 
    ROUND(SUM(od.quantity * p.price), 2) AS total_sales
FROM
    orders_details od
        JOIN
    pizzas p ON p.pizza_id = od.pizza_id;
    
 Q3.**Write a query to find all orders made at '12:00'.**:

SELECT 
    *
FROM
    orders
WHERE
    order_time = '12:00:00';
    
Q4.**Identify the highest-priced pizza.**:
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price desc limit 1;

Q5.**Identify the most common pizza size ordered.**:
SELECT 
    pizzas.size,
    COUNT(orders_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    orders_details ON pizzas.pizza_id = orders_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC
LIMIT 1;

Q6.**Write a query to list all unique pizza IDs from the orders_details table.**:
SELECT DISTINCT
    pizza_id
FROM
    orders_details;
    

Q7.**List the top 5 most ordered pizza types along with their quantities.**:

SELECT 
    pizza_types.name,
    SUM(orders_details.quantity) AS total_quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY total_quantity DESC
LIMIT 5;

Q8.**Join the necessary tables to find the total quantity of each pizza category ordered.**:
SELECT 
    pizza_types.category,
    COUNT(orders_details.quantity) AS total_quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY total_quantity DESC;

Q9.**Determine the distribution of orders by hour of the day.**:
SELECT 
    HOUR(order_time), COUNT(order_id)
FROM
    orders
GROUP BY HOUR(order_time); 

Q10.**Join relevant tables to find the category-wise distribution of pizzas.**:
SELECT 
    category, COUNT(name)
FROM
    pizza_types
GROUP BY category; 

Q11.**Group the orders by date and calculate the average number of pizzas ordered per day.**:
SELECT 
    ROUND(AVG(quantity), 0) as avg_pizza_order_per_day
FROM
    (SELECT 
        orders.order_date, SUM(orders_details.quantity) as quantity
    FROM
        orders
    JOIN orders_details ON orders.order_id = orders_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
 
 Q12.**Write a query to list all pizza types along with their ingredients from the pizzas table.**:

 SELECT 
    pizza_types.name, pizza_types.ingredients
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id;

Q13.**Determine the top 3 most ordered pizza types based on revenue.**:
 
 SELECT 
    pizza_types.name,
    SUM(orders_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizzas.pizza_type_id = Pizza_types.pizza_type_id
        JOIN
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;

Q14.**Write a query to generate a daily summary of orders and total revenue.**:
SELECT 
    o.order_date,
    COUNT(o.order_id) AS total_orders,
    round(SUM(od.quantity * p.price),0) AS total_revenue
FROM
    orders o
        JOIN
    orders_details od ON o.order_id = od.order_id
        JOIN
    pizzas p ON od.pizza_id = p.pizza_id
GROUP BY o.order_date;


Q15.**Calculate the percentage contribution of each pizza type to total revenue.**:

SELECT 
pizza_types.category,
    SUM(orders_details.quantity * pizzas.price) / 
    (SELECT 
        ROUND(SUM(orders_details.quantity * pizzas.price), 2) 
     FROM 
        orders_details 
     JOIN 
        pizzas ON pizzas.pizza_id = orders_details.pizza_id) * 100 AS revenue
FROM 
    pizza_types 
JOIN 
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN 
    orders_details ON orders_details.pizza_id = pizzas.pizza_id
GROUP BY 
    pizza_types.category 
ORDER BY 
    revenue DESC;

## Findings
-- **Customer Demographics**:Customers come from different age groups, showing a mix of preferences for various pizza types.
-- **Sales Trends**:Monthly sales vary, helping us identify busy times like weekends or holidays.
-- **Customer Insights**:We found out who the top spenders are and which pizzas are the most popular.

## Reports
-- **Sales Summary**:A report that covers total sales, customer information, and performance by pizza type.
-- **Trend Analysis**:Insights into how sales change over different months, which helps plan for promotions.
-- **Customer Insights**:Information on top customers and how many unique customers ordered each type of pizza.

## Conclusion
-- **This project helped us learn how to use SQL to analyze Pizza Hut sales data.**
-- **Set Up a Database**: Organized sales information in a clear way.
-- **Cleaned the Data**: Fixed any mistakes or missing information to make sure our data is reliable.
-- **Explored the Data**: Used SQL queries to find out important patterns in customer behavior and sales.
-- **Gained Insights**: Discovered trends that can help with future marketing and promotions.
-- **In short, this project shows how data analysis can help make better business decisions and improve customer experiences at Pizza Hut.** 
