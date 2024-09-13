# Sql Project :- Consumer Goods Ad-hoc Insights


# Consumer Goods Analysis

## Project Overview  

**Project Title**: Consumer Goods Ad-hoc Insights  
**Database**: gdb023

## Objectives
This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore and analyze consumer Goods. The project involves setting up a database, performing  data analysis, and answering (specific business questions) Ad-hoc requests through SQL queries.

## Process
**1.Set up a  database**: Create and populate a  database with the provided  data.   

1.dim_customer: contains customer-related data   
2.dim_product: contains product-related data   
3.fact_gross_price: contains gross price information for each product    
4.fact_manufacturing_cost: contains the cost incurred in the production of each product   
5.fact_pre_invoice_deductions: contains pre-invoice deductions information for each product  
6.fact_sales_monthly: contains monthly sales data for each product.

**2.Business Analysis**: Used SQL to answer specific business questions and derive insights from the  data.  

**3.Visualising the insights** : Used Power Bi to visualise the outcomes.


## Project Structure
## 1. Database Setup
Database Creation: The project starts by creating a database named gdb023 by using the provided database creation file.


## 2. Data Analysis & Findings
The following SQL queries were developed to answer Ad-hoc requests

**1.Provide the list of markets in which customer  "Atliq  Exclusive"  operates its  business in the  APAC  region.** 

 ``` sql
#  Query:
SELECT DISTINCT market 
FROM dim_customer
WHERE  customer = "Atliq Exclusive" AND region = "APAC"
ORDER BY market;
```

2.What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields,unique_products_2020 ,unique_products_2021 ,percentage_chg

``` sql
#  Query:
WITH Cte1 AS
(
SELECT COUNT(DISTINCT product_code) AS unique_products_2020
FROM fact_sales_monthly
WHERE fiscal_year = "2020"),
Cte2 AS
(
SELECT COUNT(DISTINCT product_code) AS unique_products_2021
FROM fact_sales_monthly
WHERE fiscal_year = "2021")

SELECT C1.unique_products_2020 ,C2.unique_products_2021,
ROUND(((C2.unique_products_2021 - C1.unique_products_2020)/C1.unique_products_2020 *100),2) AS percentage_chg
FROM Cte1 as C1
JOIN Cte2 as C2;
```


3.Provide a report with all the unique product counts for each  segment  and sort them in descending order of product counts.The final output contains 2 fields, segment,product_count

``` sql
#  Query:
SELECT segment,COUNT(DISTINCT product_code) AS product_counts
FROM dim_product
GROUP BY segment
ORDER BY product_counts DESC; 
```

4.Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields, segment ,product_count_2020 ,product_count_2021 ,difference 

``` sql
#  Query:
WITH Cte1 AS
(
SELECT p.segment,  COUNT(DISTINCT f.product_code) AS product_count_2020
FROM fact_sales_monthly f
JOIN dim_product p 
ON f.product_code = p.product_code
WHERE f.fiscal_year = "2020"
GROUP BY p.segment
),
Cte2 AS
(
SELECT p.segment,  COUNT(DISTINCT f.product_code) AS product_count_2021
FROM fact_sales_monthly f
JOIN dim_product p 
ON f.product_code = p.product_code
WHERE f.fiscal_year = "2021"
GROUP BY p.segment
)
SELECT C2.segment,C1.product_count_2020, C2.product_count_2021,
(C2.product_count_2021-C1.product_count_2020) AS difference
FROM Cte1 AS C1
JOIN Cte2 AS C2
	ON C1.segment = C2.segment
ORDER BY difference DESC;
```

5.Get the products that have the highest and lowest manufacturing costs.The final output should contain these fields,product_code,product,manufacturing_cost 

``` sql
#  Query:
SELECT MAX(manufacturing_cost) AS Max_cost, MIN(manufacturing_cost) AS Min_cost
FROM fact_manufacturing_cost;

SELECT p.product_code,p.product,m.manufacturing_cost 
FROM dim_product p
JOIN fact_manufacturing_cost m
ON p.product_code = m.product_code
WHERE m.manufacturing_cost = (SELECT MAX(manufacturing_cost) FROM fact_manufacturing_cost)
OR
m.manufacturing_cost = (SELECT MIN(manufacturing_cost) FROM fact_manufacturing_cost)
ORDER BY m.manufacturing_cost DESC;
```

6.Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct for the fiscal year 2021 and in the Indian market. The final output contains these fields,customer_code,customer,average_discount_percentage

``` sql
#  Query:
SELECT p.customer_code , c.customer , ROUND(AVG(p.pre_invoice_discount_pct)*100,2) AS average_discount_percentage
FROM fact_pre_invoice_deductions p
JOIN dim_customer c
ON p.customer_code = c.customer_code
WHERE p.fiscal_year='2021' AND c.market = "India"
GROUP BY c.customer_code, c.customer
ORDER BY average_discount_percentage DESC
LIMIT 5;
```

7.Get the complete report of the Gross sales amount for the customer “Atliq Exclusive” for each month.This analysis helps to get an idea of low and high-performing months and take strategic decisions.The final report contains these columns:Month,Year,Gross sales Amount

``` sql
#  Query:
SELECT 
	MONTHNAME(s.date) AS Month,
    YEAR(s.date) AS YEAR,
    CONCAT(ROUND(SUM(g.gross_price * s.sold_quantity)/1000000,2)," M" )AS Gross_sales_Amount
FROM fact_gross_price g
JOIN fact_sales_monthly s 
	ON g.product_code = s.product_code AND g.fiscal_year=s.fiscal_year
JOIN dim_customer c
	ON  s.customer_code = c.customer_code
WHERE c.customer = "Atliq Exclusive" 
GROUP BY s.date,s.fiscal_year
ORDER BY YEAR;
```

8.In which quarter of 2020, got the maximum total_sold_quantity? The final output contains these fields sorted by the total_sold_quantity,Quarter,total_sold_quantity

``` sql
#  Query:
SELECT 
CASE 
	WHEN MONTH(date) IN (9,10,11) THEN "Q1"
    WHEN MONTH(date) IN (12,1,2) THEN "Q2"
    WHEN MONTH(date) IN (3,4,5) THEN "Q3"
    WHEN MONTH(date) IN (6,7,8) THEN "Q4"
END AS Quarters, CONCAT(ROUND(SUM(sold_quantity)/1000000,2),' M') AS total_sold_quantity
FROM fact_sales_monthly
WHERE fiscal_year='2020'
GROUP BY Quarters;
```

9.Which channel helped to bring more gross sales in the fiscal year 2021 and the percentage of contribution? The final output contains these fields,channel,gross_sales_mln,percentage

``` sql
#  Query:
WITH Cte AS
(
SELECT 
	c.channel,
    ROUND((SUM(g.gross_price * s.sold_quantity)/1000000),2)  AS gross_sales_mln
FROM   fact_gross_price g 
JOIN   fact_sales_monthly s
	ON g.product_code = s.product_code AND g.fiscal_year=s.fiscal_year
JOIN dim_customer c
	ON  s.customer_code = c.customer_code
GROUP BY c.channel
)
SELECT 
	channel,
	CONCAT(gross_sales_mln,' M') as gross_sales_mln,
    ROUND((( gross_sales_mln / SUM(gross_sales_mln) OVER())*100),2) AS  percentage
FROM Cte
ORDER BY percentage DESC;
```

10.Get the Top 3 products in each division that have a high total_sold_quantity in the fiscal_year 2021? The final output contains these fields,division,product_code,product,total_sold_quantity,rank_order

``` sql
#  Query:
WITH Cte1 AS
(
SELECT p.division,p.product_code,p.product, SUM(s.sold_quantity) AS total_sold_quantity
FROM fact_sales_monthly s 
JOIN dim_product p
ON s.product_code = p.product_code
WHERE s.fiscal_year = '2021'
GROUP BY p.division,p.category,p.product_code,p.product
),
Cte2 AS
(
SELECT *,
RANK() OVER(PARTITION BY division ORDER BY  total_sold_quantity DESC ) AS rank_order
FROM Cte1 
)
SELECT *
FROM  Cte2
WHERE rank_order <=3;

```


## Reports
𝐏𝐞𝐫𝐟𝐨𝐫𝐦𝐚𝐧𝐜𝐞 𝐂𝐨𝐦𝐩𝐚𝐫𝐢𝐬𝐨𝐧:Evaluated company performance.

𝐏𝐫𝐨𝐝𝐮𝐜𝐭 𝐒𝐞𝐠𝐦𝐞𝐧𝐭𝐬:Analyzed different product segments.

𝐒𝐚𝐥𝐞𝐬 𝐂𝐨𝐦𝐩𝐚𝐫𝐢𝐬𝐨𝐧 (𝟐𝟎𝟐𝟎 𝐯𝐬. 𝟐𝟎𝟐1:Identified trends and changes in sales.

**Customer Analysis**: Identified top  customers who receive highest average pre invoice discount percentage.

𝐂𝐨𝐬𝐭 𝐀𝐧𝐚𝐥𝐲𝐬𝐢𝐬:Identified highest and lowest cost products.  
