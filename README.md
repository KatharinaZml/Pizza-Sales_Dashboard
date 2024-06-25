Pizza Sales Dashboard for 2015

Note: For details such as the outcome of SQL queries, please view the word document: Pizza Sales_Project Documentation.docx in the folder.
The README.md does not contain outcomes.

1.	About the project
Pizza Sales is a fictional company offering customers Pizza. I followed this dashboard project online (please see: Data Tutorials, 2023).  Available data for the report is from January 15th, 2015 - December 15th, 2015 (11 months).
1.1.	Dataset
The .csv sample was loaded into Microsoft SQL Server Management Studio.  Next, the dataset was checked for appropriate data types of each column and null values. 
Potential Stakeholders could include Sales or Pricing teams. Therefore, the objectives of the dashboard are to visualise Sales and Order Trends by daytime and month. Specifically, the interactive dashboard reflects the main KPIs on a hourly, daily and monthly basis in year 2015. Furthermore charts show how pizza by pizza type, category and size perform in relation to revenue and orders.

2.	Data Validation using SQL
2.1.	Key Performance Indicator (KPI) Definition using SQL
Data reflected in the PowerBi Dashboard is in line with SQL query outcomes. This ensures data is valid.
The following KPIs, definitions and SQL queries were used:

1.	Total Orders 
The total number of pizza orders placed. 

SELECT COUNT(DISTINCT(order_id) AS total_pizza_orders FROM [Pizza Database].[dbo].[pizza_sales]
COUNT(DISTINCT(order_id)
 
2.	Average number of pizzas per order
Is the average number of ordered pizzas per order (Total pizzas sold / Total Pizza Orders).

SELECT SUM(quantity) AS DECIMAL(10,2)) / COUNT(DISTINCT(order_id) DECIMAL(10,2)) AS average_pizzas_per order FROM [Pizza Database].[dbo].[pizza_sales]
 
3.	Total of sold pizzas
Is the sum of sold pizzas.

SELECT SUM(quantity) AS total_sold_pizzas 
FROM [Pizza Database].[dbo].[pizza_sales]
 
4.	Average order value
Is defined as the average value spent per customer

SELECT SUM(total_price) / COUNT(DISTINCT(order_id) as _average_order_value
FROM [Pizza Database].[dbo].[pizza_sales]
 
5.	Total Revenue
Is the sum of the total price of all ordered pizzas (Order price )

SELECT SUM(total_price) AS Total_Revenue FROM [Pizza Database].[dbo].[pizza_sales]
 
2.2.	Charts Definition using SQL

6.	Hourly trend for total pizza orders
The daily trend for total pizza orders throughout the day is displayed with a line chart. Using this visual allows to identify peak times of order activity.

SELECT DATEPART(hour, order_time) as order_hour,
COUNT(DISTINCT order_id) AS total_orders 
FROM [Pizza Database].[dbo].[pizza_sales]
GROUP BY DATEPART(hour, order_time)
ORDER BY order_hour
 

7.	Daily trend for total pizza orders

SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM [Pizza Database].[dbo].[pizza_sales]
GROUP BY DATENAME(DW, order_date)
 
8.	Monthly Trend for Orders

select DATENAME(MONTH, order_date) as Month_Name, COUNT(DISTINCT order_id) as Total_Orders
FROM [Pizza Database].[dbo].[pizza_sales]
GROUP BY DATENAME(MONTH, order_date)
 
9.	% of Sales by Pizza category: To see the sales distribution of pizza categories
This chart provides with insights into each categories' distribution to the total sales as well as popularity. 
SELECT 
    pizza_category, 
    CAST(SUM(total_price) AS DECIMAL(10, 2)) AS total_revenue,
    CAST(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM [Pizza Database].[dbo].[pizza_sales]) AS DECIMAL(10, 2)) AS percentage_pizza_category
FROM 
    [Pizza Database].[dbo].[pizza_sales]
GROUP BY 
    pizza_category;
 
10.	% of Sales by Pizza Size: To see the sales distribution of pizza size

SELECT 
    pizza_size, 
    CAST(SUM(total_price) AS DECIMAL(10, 2)) AS total_revenue, 
    (CAST(SUM(quantity) AS DECIMAL(10, 2)) / SUM(SUM(quantity)) OVER ()) * 100 AS percentage_pizza_size
FROM 
   [Pizza Database].[dbo].[pizza_sales]
GROUP BY 
    pizza_size
ORDER BY 
    pizza_size;
 
11.	Total Pizzas Sold by Pizza Category

SELECT pizza_category,
SUM(quantity) AS total_quantity_sold
FROM 
   [Pizza Database].[dbo].[pizza_sales]
GROUP BY 
    pizza_category
ORDER BY 
    pizza_category DESC;
 
12.	Top 5 Pizzas by Revenue

SELECT TOP 5 
    pizza_name,
    SUM(total_price) AS revenue
FROM 
    [Pizza Database].[dbo].[pizza_sales]
GROUP BY 
    pizza_name
ORDER BY 
    revenue DESC;
 
13.	Top 5 Pizzas by Quantity

SELECT TOP 5 
    pizza_name,
    SUM(quantity) AS total_pizza_sold
FROM 
    [Pizza Database].[dbo].[pizza_sales]
GROUP BY 
    pizza_name
ORDER BY 
    total_pizza_sold DESC;
 
14.	Top 5 Pizzas by Total Orders

SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS total_orders
FROM [Pizza Database].[dbo].[pizza_sales]
GROUP BY pizza_name
ORDER BY Total_Orders DESC;
 

3.	Data Manipulation using Power Query
Before building the visuals for the dashboard, data modifications are made for a better readability.
Example:
- pizza_size: Write out size names by replacing them with the full-size name. Example: M -> Medium
- order_date: the day name and number as well as the month name and number were extracted. 
Example:
 

3.1. Building KPIs using DAX Functions
The following KPis are created to calculate the following KPIs

(1) Total Revenue = SUM(pizza_sales[total price])
(2) Total Orders = DISTINCTCOUNT(pizza_sales[order_id])
(3) Average Order Value = [Total Revenue] / [Total Orders]
(4) Total Pizzas sold = SUM(pizza_sales[quantity])
(5) Average Pizzas per order = [Total Pizzas sold] [Total Orders]

4.	PowerBi Dashboard
4.1.	Sales Dashboard

 
Firstly, the sales dashboard shows the overall performance of pizza sales. The donut chart “By pizza size” shows that Large has the highest share of pizza sales with approx. 46%, followed by Medium size with a share of 30.5%. The Classic (approx. 27%) and Supreme (25.5%) pizza categories have the highest share of sales.
These findings could be important for sales teams. They show which pizzas are sold the most. This could be a starting point for further analyses to determine which factors determine sales success and how this can be optimised. 


4.2.	Orders Dashboard
 
The second dashboard shows information on pizza orders. 
The line charts provide insight into peak order times. "By day and time" shows that the peak times with high order volumes are Friday, and Monday at 12 pm and Thursday, Tuesday and Wednesday between 12 pm and 1 pm. In July 1935 orders were placed, while 1646 orders were received in October. This information could be valuable for incorporating seasonal fluctuations into staff and sales planning. 

References
Data Tutorials (2023): Power BI & SQL Project | Data Analyst Portfolio | End to End | Beginner to Expert | #powerbi #sql; https://www.youtube.com/watch?v=V-s8c6jMRN0&t=676s
