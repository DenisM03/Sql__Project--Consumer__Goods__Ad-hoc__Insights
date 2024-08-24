# Sql Project :- Consumer Goods Ad-hoc Insights


# Consumer Goods Analysis

## Project Overview  

**Project Title**: Consumer Goods Ad-hoc Insights  
**Database**: gdb023

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore and analyze consumer Goods. The project involves setting up a database, performing  data analysis, and answering (specific business questions) Ad-hoc requests through SQL queries.

## Objectives
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

1.Provide the list of markets in which customer  "Atliq  Exclusive"  operates its  business in the  APAC  region. 

![image](https://github.com/user-attachments/assets/b0eea44b-00c6-4fa9-9b7b-b35ad6569e66)

2.What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields,unique_products_2020 ,unique_products_2021 ,percentage_chg

![image](https://github.com/user-attachments/assets/6da65198-deec-4aab-8660-156287573bd4)

3.Provide a report with all the unique product counts for each  segment  and sort them in descending order of product counts.The final output contains 2 fields, segment,product_count

![image](https://github.com/user-attachments/assets/5e6bbdb6-9a00-4d5f-95e9-d9bd31735b0c)

4.Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields, segment ,product_count_2020 ,product_count_2021 ,difference 

![image](https://github.com/user-attachments/assets/c6cc6ea0-4c26-4ec4-85e1-9021b5847b5f)

5.Get the products that have the highest and lowest manufacturing costs.The final output should contain these fields,product_code,product,manufacturing_cost 

![image](https://github.com/user-attachments/assets/db005d8d-b9e5-452a-87de-0c7af2761e10)

6.Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct for the fiscal year 2021 and in the Indian market. The final output contains these fields,customer_code,customer,average_discount_percentage

![image](https://github.com/user-attachments/assets/a1f53574-0387-4bba-85e9-170eb7fae164)

7.Get the complete report of the Gross sales amount for the customer “Atliq Exclusive” for each month.This analysis helps to get an idea of low and high-performing months and take strategic decisions.The final report contains these columns:Month,Year,Gross sales Amount

![image](https://github.com/user-attachments/assets/15d4d61e-1f79-45e8-a354-808f6bab5045)

8.In which quarter of 2020, got the maximum total_sold_quantity? The final output contains these fields sorted by the total_sold_quantity,Quarter,total_sold_quantity

![image](https://github.com/user-attachments/assets/a6b9f1d8-56bb-491f-90c8-e61dde47bff4)

9.Which channel helped to bring more gross sales in the fiscal year 2021 and the percentage of contribution? The final output contains these fields,channel,gross_sales_mln,percentage

![image](https://github.com/user-attachments/assets/a852b6b2-8dcf-41b6-b794-8819887f15d0)

10.Get the Top 3 products in each division that have a high total_sold_quantity in the fiscal_year 2021? The final output contains these fields,division,product_code,product,total_sold_quantity,rank_order

![image](https://github.com/user-attachments/assets/621bbabe-5506-4d8b-bc53-f2a5666d6f72)


## Reports
𝐏𝐞𝐫𝐟𝐨𝐫𝐦𝐚𝐧𝐜𝐞 𝐂𝐨𝐦𝐩𝐚𝐫𝐢𝐬𝐨𝐧:Evaluated company performance.

𝐏𝐫𝐨𝐝𝐮𝐜𝐭 𝐒𝐞𝐠𝐦𝐞𝐧𝐭𝐬:Analyzed different product segments.

𝐒𝐚𝐥𝐞𝐬 𝐂𝐨𝐦𝐩𝐚𝐫𝐢𝐬𝐨𝐧 (𝟐𝟎𝟐𝟎 𝐯𝐬. 𝟐𝟎𝟐1:Identified trends and changes in sales.

**Customer Analysis**: Identified top  customers who receive highest average pre invoice discount percentage.

𝐂𝐨𝐬𝐭 𝐀𝐧𝐚𝐥𝐲𝐬𝐢𝐬:Identified highest and lowest cost products.  
