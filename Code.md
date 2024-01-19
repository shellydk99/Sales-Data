# Data Exploration
## a) Get into Database
```r[]
USE u442155536_fb_test;
SHOW TABLES;
```
## b) Get to know about table
```r[]
Desc customers;
Desc employees;
Desc offices;
Desc orderdetails;
Desc order;
Desc payment;
Desc Productlines;
Desc product;
```
# Data Cleaning
## a) Checking null or missing value
```r[]
SELECT COUNT(*) AS null_or_empty_count FROM customers WHERE customerNumber IS NULL or customerNumber = '';
SELECT COUNT(*) AS null_or_empty_count FROM employees WHERE employeeNumber IS NULL or employeeNumber = '';
SELECT COUNT(*) AS null_or_empty_count FROM offices WHERE officeCode IS NULL or officeCode = '';
SELECT COUNT(*) AS null_or_empty_count FROM orders WHERE orderNumber IS NULL or orderNumber = '';
SELECT COUNT(*) AS null_or_empty_count FROM payments WHERE checkNumber IS NULL or checkNumber = '';
SELECT COUNT(*) AS null_or_empty_count FROM productlines WHERE productLine IS NULL or productLine = '';
SELECT COUNT(*) AS null_or_empty_count FROM products WHERE productCode IS NULL or productCode = '';
```
There is no null or missing value founded
## b) Checking for duplicate
```r[]
SELECT customerNumber, COUNT(*) FROM customers GROUP BY customerNumber HAVING COUNT(*) > 1;
SELECT employeeNumber, COUNT(*) FROM employees GROUP BY employeeNumber HAVING COUNT(*) > 1;
SELECT officeCode, COUNT(*) FROM offices GROUP BY officeCode HAVING COUNT(*) > 1;
SELECT orderNumber, COUNT(*) FROM orders GROUP BY orderNumber HAVING COUNT(*) > 1;
SELECT checkNumber, COUNT(*) FROM payments GROUP BY checkNumber HAVING COUNT(*) > 1;
SELECT productLine, COUNT(*) FROM productlines GROUP BY productLine HAVING COUNT(*) > 1;
SELECT productCode, COUNT(*) FROM products GROUP BY productCode HAVING COUNT(*) > 1;
```
## c) Count and group by length
```r[]
SELECT LENGTH(customerNumber) AS Length, COUNT(*) AS Count FROM customers GROUP BY Length;
SELECT LENGTH(employeeNumber) AS Length, COUNT(*) AS Count FROM employees GROUP BY Length;
SELECT LENGTH(officeCode) AS Length, COUNT(*) AS Count FROM offices GROUP BY Length;
SELECT LENGTH(orderNumber) AS Length, COUNT(*) AS Count FROM orders GROUP BY Length;
SELECT LENGTH(checkNumber) AS Length, COUNT(*) AS Count FROM payments GROUP BY Length;
SELECT LENGTH(productLine) AS Length, COUNT(*) AS Count FROM productlines GROUP BY Length;
SELECT LENGTH(productCode) AS Length, COUNT(*) AS Count FROM products GROUP BY Length;
```
# Data Preparation
## 1) Customer behaviour
```r[]
SELECT
    c.customerName, c.country, c.creditLimit,
    o.orderDate, o.status,
    od.quantityOrdered, od.priceEach,
    p.productName, p.productLine
FROM customers c
JOIN orders o ON c.customerNumber = o.customerNumber
JOIN orderdetails od ON o.orderNumber = od.orderNumber
JOIN products p ON od.productCode = p.productCode
```
## 2) Product sale
```r[]
SELECT
  pl.productLine,
  p.productName, p.productVendor, p.quantityInStock,
  od.quantityOrdered,
  o.orderDate, o.status,
  py.amount
From productlines pl
JOIN products p ON pl.productLine = p.productLine
JOIN orderdetails od ON p.productCode = od.productCode
JOIN orders o ON od.orderNumber = o.orderNumber
JOIN payments py ON o.customerNumber = py.customerNumber
```
## 3) Employee monitoring
```r[]
SELECT
    CONCAT_WS(' ', e.lastName, e.firstName) AS name, e.jobTitle,
    os.country,
    o.orderDate,
    od.quantityOrdered,
    p.productName, p.productLine,
    py.amount
FROM employees e
JOIN customers c ON e.employeeNumber = c.salesRepEmployeeNumber
JOIN offices os ON e.officeCode = os.officeCode
JOIN orderdetails od ON o.orderNumber = od.orderNumber
JOIN products p ON od.productCode = p.productCode
JOIN payments py ON o.customerNumber = py.customerNumber
