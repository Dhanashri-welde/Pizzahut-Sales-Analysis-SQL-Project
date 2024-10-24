# Pizzahut-Sales-Analysis-SQL-Project
## Project Overview
**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `pizzahut`
This project is designed to demonstrate SQL skills and techniques used by data analysts to explore, clean, and analyze Pizza Hut sales data. It involves setting up a Pizza Hut sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is perfect for beginners looking to build a solid foundation in data analysis and gain practical experience with SQL
## Objectives
* Set up a Pizza Hut Sales Database: Create and populate a database with the provided sales data.
* Data Cleaning: Identify and remove any records with missing or null values.
* Exploratory Data Analysis (EDA): Perform basic EDA to understand the dataset.
* Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup
**Database Creation**:The project begins by creating a database named pizzahut.
**Table Creation**: Several tables are created to store sales data:
**orders**: Stores general order information.
**order_details**: Stores details of each pizza in an order.
**pizzas**: Contains information about different pizzas.
**pizza_types**: Categorizes pizzas.

create database pizzahut;

create table orders(order_id int primary key,
order_date datetime,
order_time time);

create table orders_details(order_details_id int primary key,
order_id int,
pizza_id text,
quantity int);

create table pizza_types(pizza_type_id text,
name text,
category text,
ingredients text);

create table pizzas(pizza_id text,
pizza_type_id text,
size text,
price double);
'''
### 2. Data Exploration & Cleaning
 **Record Count**:Determine the total number of records in the orders table.
 **Pizza Count**:Find out how many unique customers are in the dataset.
 **Pizza_types Count**:Identify all unique pizza types in the dataset.
 **Null Value Check**:Check for any null values in the orders, orders_details, pizza_types, and pizzas tables.
 **Data Deletion**:Delete records with null values.
'''
sql 
SELECT COUNT(*) AS TotalOrders FROM orders;
SELECT COUNT(DISTINCT pizza_id) AS TotalPizzas FROM pizzas;
SELECT COUNT(DISTINCT pizza_type_id) AS TotalPizzaTypes FROM pizza_types;

SELECT 
    *
FROM
    orders
WHERE
    order_id IS NULL OR order_date IS NULL
        OR order_time IS NULL;

SELECT * FROM orders_details
WHERE order_details_id IS NULL OR order_id IS NULL OR pizza_id IS NULL OR quantity IS NULL;

SELECT * FROM pizza_types
WHERE pizza_type_id IS NULL OR name IS NULL OR category IS NULL OR ingredients IS NULL;

DELETE FROM orders
WHERE order_id IS NULL OR order_date IS NULL OR order_time IS NULL;
'''

### 3. Data Analysis & Findings

