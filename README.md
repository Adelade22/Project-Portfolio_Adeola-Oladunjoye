/*PIZZA SALES SQL QUERIES
We have been hired by a pizza restaurant to use data-driven approaches to improve their Operations, Sales, Products and Revenue.
To gain insights into the pizza sales, we imported their raw data into a Pizza Database on MS SQL Server to generate our pizza 
sales table. The dataset contains a year’s (2015) worth of sales.
We used SQL queries to examine crucial metrics within our pizza sales data, enabling us to understand the business performance better. 
Specifically, our focus was on calculating the following metrics: Total Revenue, Average Order Value, Total Pizzas Sold, Total Orders, Average Pizzas per Order, alongside delving into 
other potential insights within the dataset. */

/*
A. KPI’s
1. Total Revenue:
This SQL query calculates the total revenue from pizza sales.
*/
SELECT 
    SUM(total_price) AS Total_Revenue    -- Selects the sum of the total_price column and aliases it as Total_Revenue
FROM 
    pizza_sales;  -- Specifies the table from which to retrieve data, in this case, pizza_sales

/*
2. Average Order Value
This SQL query calculates the average order value from pizza sales. */
SELECT 
    (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value  -- Calculates the average order value by dividing the sum of total_price by the count of distinct order_id, and aliases it as Avg_order_Value
FROM 
    pizza_sales;  -- Specifies the table from which to retrieve data, in this case, pizza_sales

/*
3. Total Pizzas Sold
This SQL query calculates the total number of pizzas sold from the pizza sales data. */
SELECT 
    SUM(quantity) AS Total_pizza_sold   -- Calculates the sum of the quantity column, representing the total number of pizzas sold, and aliases it as Total_pizza_sold
FROM 
    pizza_sales;  -- Specifies the table from which to retrieve data, in this case, pizza_sales

/*
4. Total Orders
This SQL query calculates the total number of distinct orders from the pizza sales data. */
SELECT 
    COUNT(DISTINCT order_id) AS Total_Orders   -- Counts the distinct order_id values, representing the total number of orders, and aliases it as Total_Orders
FROM 
    pizza_sales;  -- Specifies the table from which to retrieve data, in this case, pizza_sales

/*
5. Average Pizzas Per Order
This SQL query calculates the average number of pizzas per order from the pizza sales data. */
SELECT 
    CAST( -- Casts the result of the division operation to ensure precision
        CAST(SUM(quantity) AS DECIMAL(10,2)) /  -- Casts the sum of quantity as DECIMAL with precision 10 and scale 2
        CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2))  -- Casts the count of distinct order_id as DECIMAL with precision 10 and scale 2
    AS DECIMAL(10,2))  -- Casts the result of the division as DECIMAL with precision 10 and scale 2, representing the average number of pizzas per order
AS Avg_Pizzas_per_order  -- Aliases the result as Avg_Pizzas_per_order
FROM
    pizza_sales;   -- Specifies the table from which to retrieve data, in this case, pizza_sales

/*
B. Daily Trend for Total Orders
This SQL query retrieves the count of distinct orders for each day of the week from the pizza sales data. */
SELECT 
    DATENAME(DW, order_date) AS order_day,  -- Retrieves the day of the week (e.g., Monday, Tuesday) from the order_date column and aliases it as order_day
    COUNT(DISTINCT order_id) AS total_orders  -- Counts the distinct order_id values for each day of the week, representing the total number of orders, and aliases it as total_orders
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    DATENAME(DW, order_date);  -- Groups the results by the day of the week extracted from the order_date column

/*
C. Monthly Trend for Orders
This SQL query retrieves the count of distinct orders for each month from the pizza sales data. */
SELECT 
    DATENAME(MONTH, order_date) AS Month_Name,  -- Retrieves the name of the month (e.g., January, February) from the order_date column and aliases it as Month_Name
    COUNT(DISTINCT order_id) AS Total_Orders  -- Counts the distinct order_id values for each month, representing the total number of orders, and aliases it as Total_Orders
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    DATENAME(MONTH, order_date);  -- Groups the results by the month extracted from the order_date column

/*
D. % of Sales by Pizza Category
This SQL query calculates the total revenue and percentage of total revenue for each pizza category from the pizza sales data. */
SELECT 
    pizza_category,  -- Selects the pizza category column
    CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,  -- Calculates the total revenue for each pizza category and casts it as DECIMAL with precision 10 and scale 2
    CAST( -- Casts the result of the percentage calculation to ensure precision
        SUM(total_price) * 100 / (  -- Calculates the percentage of total revenue for each pizza category
            SELECT SUM(total_price)  -- Calculates the total revenue of all pizza sales
            FROM pizza_sales
        ) 
    AS DECIMAL(10,2)) AS PCT  -- Casts the result of the percentage calculation as DECIMAL with precision 10 and scale 2, and aliases it as PCT
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_category;  -- Groups the results by the pizza category

/*
E. % of Sales by Pizza Size
This SQL query calculates the total revenue and percentage of total revenue for each pizza size from the pizza sales data. */
SELECT 
    pizza_size,  -- Selects the pizza size column
    CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,  -- Calculates the total revenue for each pizza size and casts it as DECIMAL with precision 10 and scale 2
    CAST( -- Casts the result of the percentage calculation to ensure precision
        SUM(total_price) * 100 / ( -- Calculates the percentage of total revenue for each pizza size
            SELECT SUM(total_price) -- Calculates the total revenue of all pizza sales
            FROM pizza_sales
        ) 
    AS DECIMAL(10,2)) AS PCT  -- Casts the result of the percentage calculation as DECIMAL with precision 10 and scale 2, and aliases it as PCT
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_size  -- Groups the results by the pizza size
ORDER BY 
    pizza_size;  -- Orders the results by pizza size

/*
F. Total Pizzas Sold by Pizza Category
This SQL query calculates the total quantity sold for each pizza category for the month of February from the pizza sales data. */
SELECT 
    pizza_category,  -- Selects the pizza category column
    SUM(quantity) as Total_Quantity_Sold -- Calculates the total quantity sold for each pizza category and aliases it as Total_Quantity_Sold
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
WHERE 
    MONTH(order_date) = 2  -- Filters the data to include only orders from the month of February
GROUP BY 
    pizza_category  -- Groups the results by the pizza category
ORDER BY 
    Total_Quantity_Sold DESC;  -- Orders the results by total quantity sold in descending order

/*
G. Top 5 Pizzas by Revenue
This SQL query retrieves the top 5 pizza names based on total revenue from the pizza sales data. */
SELECT 
    Top 5 pizza_name,  -- Selects the top 5 pizza names
    SUM(total_price) AS Total_Revenue  -- Calculates the total revenue for each pizza name and aliases it as Total_Revenue
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_name  -- Groups the results by pizza name
ORDER BY 
    Total_Revenue DESC;  -- Orders the results by total revenue in descending order

/*
H. Bottom 5 Pizzas by Revenue
This SQL query retrieves the bottom 5 pizza names based on total revenue from the pizza sales data. */
SELECT 
    Top 5 pizza_name,  -- Selects the bottom 5 pizza names
    SUM(total_price) AS Total_Revenue  -- Calculates the total revenue for each pizza name and aliases it as Total_Revenue
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_name  -- Groups the results by pizza name
ORDER BY 
    Total_Revenue ASC;  -- Orders the results by total revenue in ascending order

/*
I. Top 5 Pizzas by Quantity
This SQL query retrieves the top 5 pizza names based on the total quantity sold from the pizza sales data. */
SELECT 
    Top 5 pizza_name,  -- Selects the top 5 pizza names
    SUM(quantity) AS Total_Pizza_Sold  -- Calculates the total quantity sold for each pizza name and aliases it as Total_Pizza_Sold
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_name  -- Groups the results by pizza name
ORDER BY 
    Total_Pizza_Sold DESC;  -- Orders the results by total quantity sold in descending order

/*
J. Bottom 5 Pizzas by Quantity
This SQL query retrieves the bottom 5 pizza names based on the total quantity sold from the pizza sales data. */
SELECT 
    TOP 5 pizza_name,  -- Selects the bottom 5 pizza names
    SUM(quantity) AS Total_Pizza_Sold  -- Calculates the total quantity sold for each pizza name and aliases it as Total_Pizza_Sold
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_name  -- Groups the results by pizza name
ORDER BY 
    Total_Pizza_Sold ASC;  -- Orders the results by total quantity sold in ascending order

/*
K. Top 5 Pizzas by Total Orders
This SQL query retrieves the top 5 pizza names based on the total number of distinct orders they appeared in from the pizza sales data. */
SELECT 
    TOP 5 pizza_name,  -- Selects the top 5 pizza names
    COUNT(DISTINCT order_id) AS Total_Orders  -- Calculates the total number of distinct orders for each pizza name and aliases it as Total_Orders
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_name  -- Groups the results by pizza name
ORDER BY 
    Total_Orders DESC;  -- Orders the results by total number of distinct orders in descending order

/*
L. Bottom 5 Pizzas by Total Orders
This SQL query retrieves the bottom 5 pizza names based on the total number of distinct orders they appeared in from the pizza sales data. */
SELECT 
    TOP 5 pizza_name, -- Selects the bottom 5 pizza names
    COUNT(DISTINCT order_id) AS Total_Orders  -- Calculates the total number of distinct orders for each pizza name and aliases it as Total_Orders
FROM 
    pizza_sales  -- Specifies the table from which to retrieve data, in this case, pizza_sales
GROUP BY 
    pizza_name  -- Groups the results by pizza name
ORDER BY 
    Total_Orders ASC;  -- Orders the results by total number of distinct orders in ascending order

/*
NOTE
If you want to apply the pizza_category or pizza_size filters to the above queries you can use WHERE clause. Follow some of below examples
SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
WHERE pizza_category = 'Classic'
GROUP BY pizza_name
ORDER BY Total_Orders ASC */

--Please refer to the Power BI frontend dashboard (Pizza Sales Report_Power BI) for a comprehensive, end to end perspective of the Pizza Sales project.
