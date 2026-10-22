CREATE DATABASE IF NOT EXISTS RetailSalesDB;

USE RetailSalesDB;

CREATE TABLE Sales (
Sale_ID INT PRIMARY KEY,
Product_Name VARCHAR(40) NOT NULL,
Category VARCHAR(25) NOT NULL,
Region VARCHAR(20) NOT NULL,
Quantity INT NOT NULL CHECK (Quantity > 0),
Unit_Price DECIMAL(10,2) NOT NULL CHECK (Unit_Price >= 0)
);

INSERT INTO Sales VALUES
(1,'Laptop','Electronics','South',2,45000),
(2,'Mouse','Electronics','North',10,600),
(3,'Chair','Furniture','South',4,3500),
(4,'Desk','Furniture','East',3,8000),
(5,'Notebook','Stationery','West',20,80),
(6,'Pen','Stationery','North',50,20),
(7,'Monitor','Electronics','East',5,12000),
(8,'Chair','Furniture','North',6,3200),
(9,'Notebook','Stationery','South',30,75),
(10,'Keyboard','Electronics','West',8,1200),
(11,'Desk','Furniture','West',2,8500),
(12,'Pen','Stationery','East',40,25);

SELECT COUNT(*) AS Total_Records FROM Sales;

SELECT SUM(Quantity) AS Total_Units FROM Sales;

SELECT SUM(Quantity * Unit_Price) AS Total_Revenue FROM Sales;

SELECT ROUND(AVG(Unit_Price), 2) AS Avg_Price FROM Sales;

SELECT MIN(Unit_Price) AS Lowest_Price, MAX(Unit_Price) AS Highest_Price FROM Sales;

SELECT COUNT(DISTINCT Category) AS Categories FROM Sales;

SELECT Category, COUNT(*) AS Sale_Records FROM Sales GROUP BY Category ORDER BY
Category;

SELECT Category, SUM(Quantity) AS Units_Sold FROM Sales GROUP BY Category ORDER BY
Category;

SELECT Category, SUM(Quantity * Unit_Price) AS Revenue FROM Sales GROUP BY Category
ORDER BY Category;

SELECT Region, SUM(Quantity * Unit_Price) AS Revenue FROM Sales GROUP BY Region ORDER
BY Revenue DESC, Region;

SELECT Category, SUM(Quantity) AS Units_Sold FROM Sales GROUP BY Category HAVING SUM(
Quantity) > 20 ORDER BY Category;

SELECT Region, SUM(Quantity * Unit_Price) AS Revenue FROM Sales GROUP BY Region
HAVING SUM(Quantity * Unit_Price) > 30000 ORDER BY Revenue DESC;

SELECT Category, SUM(Quantity) AS Units_Sold FROM Sales WHERE Region IN ('North',
'South') GROUP BY Category HAVING SUM(Quantity) >= 10 ORDER BY Category;

SELECT Category, Region, SUM(Quantity) AS Units_Sold FROM Sales GROUP BY Category,
Region ORDER BY Category, Region;

SELECT Category, SUM(Quantity * Unit_Price) AS Revenue FROM Sales GROUP BY Category
ORDER BY Revenue DESC, Category LIMIT 1;

SELECT Category, ROUND(AVG(Quantity), 2) AS Avg_Quantity FROM Sales GROUP BY Category
ORDER BY Category;

SELECT COUNT(*) AS Transactions, SUM(Quantity) AS Units,
SUM(Quantity * Unit_Price) AS Revenue
FROM Sales;
