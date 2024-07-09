# Exploratory Data Analysis of Northwind Traders Sales Data

## Changing the table names

```sql
ALTER TABLE customers
RENAME TO the_dataminant_customers;

ALTER TABLE employees
RENAME TO the_dataminant_employees;

ALTER TABLE products
RENAME TO the_dataminant_products;

ALTER TABLE region
RENAME TO the_dataminant_region;

ALTER TABLE orders
RENAME TO the_dataminant_orders;

ALTER TABLE order_details
RENAME TO the_dataminant_order_details;

ALTER TABLE shippers
RENAME TO the_dataminant_shippers;

ALTER TABLE suppliers
RENAME TO the_dataminant_suppliers;

```
 &nbsp;


  ## 1. What are the top 10 selling product of Northwind Traders?
  
  ```sql
SELECT p.productname, SUM(o.quantity) AS Total_Quantity_Sold
FROM the_dataminant_products p
JOIN the_dataminant_order_details o
ON p.productid = o.productid
GROUP BY p.productid
ORDER BY Total_Quantity_Sold DESC
LIMIT 10
```
![top 10 product](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/ce05a3824e62f329e5f649aaaf7b482065b23e7d/Exploratory%20Data%20Analysis/Questions/What%20are%20the%20top%2010%20selling%20product%20of%20Northwind%20Traders.jpg)
  &nbsp;
  
   &nbsp;

## 2. Classify customers based on their sales volumes (Grade A-C)

```sql
SELECT c.companyname, ROUND(SUM((od.quantity * od.unitprice ))) AS total_sales_amount,
    CASE
        WHEN SUM((od.quantity * od.unitprice)) >= 30000 THEN 'A'
        WHEN SUM((od.quantity * od.unitprice)) < 30000 AND SUM((od.quantity * od.unitprice)) >=20000 THEN 'B'
    ELSE 'C'
END AS customer_grade_score
FROM the_dataminant_customers c
INNER JOIN the_dataminant_orders o ON c.customerid = o.customerid
INNER JOIN the_dataminant_order_details od ON o.orderid = od.orderid
INNER JOIN the_dataminant_products p ON od.productid = p.productid
GROUP BY c.companyname
ORDER BY total_sales_amount DESC
```
![classfy customer](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/4b3d37e982e9961f1a4e785a55d67139fbfda5fd/Exploratory%20Data%20Analysis/Questions/Classify%20customers%20based%20on%20their%20sales%20volumes%20(Grade%20A-C)%201.jpg)
![classfy customer](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/4b3d37e982e9961f1a4e785a55d67139fbfda5fd/Exploratory%20Data%20Analysis/Questions/Classify%20customers%20based%20on%20their%20sales%20volumes%20(Grade%20A-C)%202.jpg)
