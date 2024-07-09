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
LIMIT 10;
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
ORDER BY total_sales_amount DESC;
```
![classfy customer](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/4b3d37e982e9961f1a4e785a55d67139fbfda5fd/Exploratory%20Data%20Analysis/Questions/Classify%20customers%20based%20on%20their%20sales%20volumes%20(Grade%20A-C)%201.jpg)
![classfy customer](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/4b3d37e982e9961f1a4e785a55d67139fbfda5fd/Exploratory%20Data%20Analysis/Questions/Classify%20customers%20based%20on%20their%20sales%20volumes%20(Grade%20A-C)%202.jpg)
&nbsp;
  
   &nbsp;

## 3. Which customer made the most purchase from Northwind Traders?

```sql
SELECT c.customerid, c.contactname, c.companyname, ROUND(SUM(od.quantity + p.unitprice)) AS total_amount_spent
FROM the_dataminant_customers c
JOIN the_dataminant_orders o ON c.customerid = o.customerid
JOIN the_dataminant_order_details od ON o.orderid = od.orderid
JOIN the_dataminant_products p ON od.productid = p.productid
GROUP BY c.customerid, c.contactname, c.companyname
ORDER BY total_amount_spent DESC
LIMIT 1;
```
![best customer](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/c3901fed7b218052a20d170030d04ab738f7b14c/Exploratory%20Data%20Analysis/Questions/Which%20customer%20made%20the%20most%20purchase%20from%20Northwind%20Traders.jpg)
&nbsp;
  
   &nbsp;

## 4. What is the total sales revenue?

```sql
SELECT  ROUND(SUM(unitprice * quantity)) AS total_revenue
FROM the_dataminant_order_details;
```
![total sales revenue](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/c3901fed7b218052a20d170030d04ab738f7b14c/Exploratory%20Data%20Analysis/Questions/What%20is%20the%20total%20sales%20revenue.jpg)
&nbsp;
  
   &nbsp;

## 5. What is the  sales revenue by product category?

```sql
SELECT c.categoryname, ROUND(SUM(od.unitprice * od.quantity)) AS total_revenue 
FROM the_dataminant_products p
JOIN categories c  ON  p.categoryid = c.categoryid
JOIN the_dataminant_order_details od  ON  p.productid = od.productid
JOIN the_dataminant_orders o  ON  od.orderid = o.orderid
GROUP BY c.categoryname
ORDER BY total_revenue DESC;
```
![sale by product](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/c3901fed7b218052a20d170030d04ab738f7b14c/Exploratory%20Data%20Analysis/Questions/What%20is%20the%20%20sales%20revenue%20by%20product%20category.jpg)
&nbsp;
  
   &nbsp;

## 6. What is the company's sales performances by cities?

```sql
SELECT c.city, c.country, ROUND(SUM(od.unitprice * od.quantity)) AS total_sales_amount
FROM the_dataminant_customers c
JOIN the_dataminant_orders o ON c.customerid = o.customerid
JOIN the_dataminant_order_details od ON o.orderid = od.orderid
GROUP BY c.country, c.city
ORDER BY country DESC;
```
![performance by city](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/c3901fed7b218052a20d170030d04ab738f7b14c/Exploratory%20Data%20Analysis/Questions/What%20is%20the%20company's%20sales%20performances%20by%20cities.jpg)
&nbsp;
  
   &nbsp;

## 7. What is the company's sales performances by country?

```sql
SELECT c.country, ROUND(SUM(od.unitprice * od.quantity)) AS total_sales_amount
FROM the_dataminant_customers c
JOIN the_dataminant_orders o ON c.customerid = o.customerid
JOIN the_dataminant_order_details od ON o.orderid = od.orderid
JOIN the_dataminant_products p ON od.productid = p.productid
GROUP BY c.country
ORDER BY total_sales_amount DESC;
```
![performance by country](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/c3901fed7b218052a20d170030d04ab738f7b14c/Exploratory%20Data%20Analysis/Questions/What%20is%20the%20company's%20sales%20performances%20by%20countries.jpg)
&nbsp;
  
   &nbsp;

## 8. Who is the best performing sales representative in terms of revenue generating?

```sql
SELECT e.employeeid, firstname || ' ' || lastname AS full_name, ROUND(SUM(od.unitprice * od.quantity)) AS total_revenue
FROM the_dataminant_orders o 
JOIN the_dataminant_order_details od ON o.orderid = od.orderid
JOIN the_dataminant_employees e ON o.employeeid = e.employeeid
GROUP BY e.employeeid, full_name
ORDER BY country DESC
LIMIT 1;
```
![besy sales rep](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/35a9678bd7089a8e98670e6870cdd3e9fcc9f1e5/Exploratory%20Data%20Analysis/Questions/Who%20is%20the%20best%20performing%20sales%20representative%20in%20terms%20of%20revenue%20generating.jpg)
&nbsp;
  
   &nbsp;

## 9. Provide list and full details about all employees and the managers they report to?

```sql
SELECT
    e.firstname || ' ' || e.lastname AS employee_full_name,	e.title AS employee_title,
	EXTRACT(YEAR FROM AGE(e.hiredate, e.birthdate)) AS employee_age,
	CONCAT(m.firstname, ' ', m.lastname) AS manager_full_name, 	m.title AS manager_title
FROM the_dataminant_employees e
INNER JOIN the_dataminant_employees m ON m.employeeid = e.reportsto
ORDER BY employee_age, employee_full_name;
```
![employees and managers](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/d7a58e14e5d23a1b2b4d310a939f0c8e4667a4b5/Exploratory%20Data%20Analysis/Questions/Provide%20list%20and%20full%20details%20about%20all%20employees%20and%20the%20managers%20they%20report%20to.jpg)
&nbsp;
  
   &nbsp;

## 10. What is the average days between the order date and the shipping date, also the total number of orders for year 1998.
##    Show orders with average days between the order date and the shipping date is greater or equal 5 days.
##    The total number of orders is greater than 10 orders. 

```sql
WITH cte_avg_days AS (
	SELECT
		   shipcountry,
		   ROUND(AVG(EXTRACT(DAY FROM (shippeddate - orderdate) * INTERVAL '1 DAY')))
	    AS average_days_between_order_shipping,
     COUNT(*) AS total_number_orders
	FROM the_dataminant_orders
	WHERE EXTRACT(YEAR FROM orderdate) = 1998
	GROUP BY shipcountry
	ORDER BY shipcountry
	
)
	
SELECT * FROM cte_avg_days
WHERE average_days_between_order_shipping >= 5
AND total_number_orders > 10;

![avg day btw od and sd](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/64511d017b014b6c4cf0a7dc31ac992adec0f9ba/Exploratory%20Data%20Analysis/Questions/What%20is%20the%20average%20days%20between%20the%20order%20date%20and%20the%20shipping%20date%2C%20also%20the%20total%20number%20of%20orders%20for%20year%201998%20-%20Question.jpg)
![avg day btw od and sd](https://github.com/Dataminant/Exploratory-Data-Analysis-of-Northwind-Traders-Sales-Data/blob/64511d017b014b6c4cf0a7dc31ac992adec0f9ba/Exploratory%20Data%20Analysis/Questions/What%20is%20the%20average%20days%20between%20the%20order%20date%20and%20the%20shipping%20date%2C%20also%20the%20total%20number%20of%20orders%20for%20year%201998%20-%20Answer.jpg)
