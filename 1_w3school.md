# Part 1: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK?

Query:
SELECT * FROM Customers WHERE Country = 'UK';

Answer:
CustomerID	CustomerName	ContactName	Address	City	PostalCode	Country
4	Around the Horn	Thomas Hardy	120 Hanover Sq.	London	WA1 1DP	UK
11	B's Beverages	Victoria Ashworth	Fauntleroy Circus	London	EC2 5NT	UK
16	Consolidated Holdings	Elizabeth Brown	Berkeley Gardens 12 Brewery	London	WX1 6LT	UK
19	Eastern Connection	Ann Devon	35 King George	London	WX3 6FW	UK
38	Island Trading	Helen Bennett	Garden House Crowther Way	Cowes	PO31 7PJ	UK
53	North/South	Simon Crowther	South House 300 Queensbridge	London	SW7 1RZ	UK
72	Seven Seas Imports	Hari Kumar	90 Wadhurst Rd.	London	OX15 4NB	UK



2. What is the name of the customer who has the most orders?

Query:
SELECT CustomerName, COUNT(OrderId) AS Order_Count
FROM Orders LEFT JOIN Customers 
ON Orders.CustomerID = Customers.CustomerID
GROUP BY CustomerName
ORDER BY COUNT(*) DESC
LIMIT 1;

Answer:
Ernst Handel is the customer with the most orders



3. Which supplier has the highest average product price?

Query:
SELECT SupplierName, AVG(Price) AS Average_Price
FROM Products LEFT JOIN Suppliers
ON Products.SupplierID = Suppliers.SupplierID
GROUP BY SupplierName
ORDER BY AVG(PRICE) DESC
LIMIT 1;

Answer:
Aux joyeux ecclésiastiques has the highest average product price of 140.75



4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)

Query:
SELECT COUNT(DISTINCT Country) as Num_of_Countries FROM Customers;

Answer:
The customers are from 21 different countries.



5. What category appears in the most orders?

Query:
WITH Cat_Ord AS (
SELECT DISTINCT Categories.CategoryName, Products.CategoryID, OrderDetails.OrderID
FROM OrderDetails LEFT JOIN Products
ON OrderDetails.ProductID = Products.ProductID
LEFT JOIN Categories
ON Products.CategoryID = Categories.CategoryID
)

SELECT CategoryName, COUNT(DISTINCT OrderID) as Order_Count
FROM Cat_Ord
GROUP BY CategoryName
ORDER BY COUNT(DISTINCT OrderID) DESC
LIMIT 1;

Answer:
Beverages appears in the most orders with 80 total.



6. What was the total cost for each order?

Query:
WITH Ord_Items AS (
SELECT OrderDetails.OrderID, (OrderDetails.Quantity * Products.Price) AS Item_Total
FROM OrderDetails LEFT JOIN Products
ON OrderDetails.ProductID = Products.ProductID
)

SELECT OrderID, SUM(Item_Total) as Total_Order_Price
FROM Ord_Items
GROUP BY OrderID

An



7. Which employee made the most sales (by total price)?




8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)

Query:
SELECT EmployeeID, LastName, FirstName
FROM Employees
WHERE Notes LIKE '%BS%';

Answer:
EmployeeID	LastName	FirstName
3	Leverling	Janet
5	Buchanan	Steven

Steven Buchanan has a BSC degree rather than a BS degree but since it is also a bachelor of science I included it.



9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)

Query:
SELECT SupplierName, AVG(Price) AS Average_Price
FROM Products LEFT JOIN Suppliers
ON Products.SupplierID = Suppliers.SupplierID
GROUP BY SupplierName
HAVING COUNT(ProductID) >= 3
ORDER BY AVG(PRICE) DESC
LIMIT 1;

Answer:
Tokyo Traders is the supplier with three or more products that has the highest average product price of 46.