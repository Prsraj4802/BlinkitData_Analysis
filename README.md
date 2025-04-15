# BlinkitData_Analysis
The analysis cleans and aggregates Blinkit sales data to calculate key metrics like total sales, average sales, and sales by categories such as fat content, item type, and outlet size. SQL queries are used to generate insights on sales performance by various outlet characteristics.

##  What information is available in the blinkit_data table?
SelECT * FROM blinkit_data

## How many rows are there in the blinkit_data table?
select count(*) from blinkit_data

## How are variations in the Item_Fat_Content column being standardized?
update blinkit_data
set Item_Fat_Content = 
case 
when Item_Fat_Content in ('LF','low fat') then 'Low Fat'
when Item_Fat_Content = 'reg' then 'Regular'
else Item_Fat_Content
end

## What distinct values exist in the Item_Fat_Content column after cleaning the data?
select distinct(Item_Fat_Content) from blinkit_data

##  What is the total sales in millions from the blinkit_data table?
select CAST(SUM(Total_Sales)/ 1000000 as decimal(10,2)) as Total_Sales_Million
from blinkit_data
--WHERE Outlet_Establishment_Year = 2022
--WHERE Item_Fat_Content = 'Low Fat'

##  What is the average sales amount from the blinkit_data table?
select CAST(avg(Total_Sales) AS DECIMAL(10,1)) AS Avg_Sales FROM blinkit_data

##  What is the average sales amount from the blinkit_data table?
SELECT COUNT(*) AS No_Of_Items FROM blinkit_data

##  What is the average sales amount from the blinkit_data table?
SELECT CAST(AVG(Rating)AS DECIMAL(10,2)) AS Avg_Rating FROM blinkit_data

##  What are the total sales, average sales, number of items, and average rating for each Item_Fat_Content category, ordered by total sales in thousands?
SELECT Item_Fat_Content, 
	CAST(SUM(Total_Sales)/1000 AS DECIMAL(10,2)) AS Total_Sales_Thousands,
	CAST(avg(Total_Sales) AS DECIMAL(10,1)) AS Avg_Sales,
	COUNT(*) AS No_Of_Items,
	CAST(AVG(Rating)AS DECIMAL(10,2)) AS Avg_Rating 
FROM blinkit_data
--WHERE Outlet_Establishment_Year = 2020 
GROUP BY Item_Fat_Content
ORDER BY Total_Sales_Thousands DESC

## What are the top 5 Item_Type categories based on total sales, and what are their corresponding average sales, number of items, and average ratings?
SELECT TOP 5 Item_Type, 
	CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
	CAST(avg(Total_Sales) AS DECIMAL(10,1)) AS Avg_Sales,
	COUNT(*) AS No_Of_Items,
	CAST(AVG(Rating)AS DECIMAL(10,2)) AS Avg_Rating 
FROM blinkit_data
--WHERE Outlet_Establishment_Year = 2020 
GROUP BY Item_Type
ORDER BY Total_Sales DESC

## How do the total sales, average sales, number of items, and average ratings vary by both Outlet_Location_Type and Item_Fat_Content?
SELECT Outlet_Location_Type, Item_Fat_Content, 
	CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
	CAST(avg(Total_Sales) AS DECIMAL(10,1)) AS Avg_Sales,
	COUNT(*) AS No_Of_Items,
	CAST(AVG(Rating)AS DECIMAL(10,2)) AS Avg_Rating 
FROM blinkit_data
--WHERE Outlet_Establishment_Year = 2020 
GROUP BY Outlet_Location_Type, Item_Fat_Content
ORDER BY Total_Sales DESC
