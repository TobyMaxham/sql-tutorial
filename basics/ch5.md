# SQL Tutorial: The Basics
# Chapter 5: Joins


##CROSS JOIN
This is the simpliest JOIN variation. In this case every record of the first table will merge with every record of the second table.
Example:
```sql
-- 165935 (2155 * 77)
SELECT COUNT(*) FROM [dbo].[Products], [dbo].[Order Details]

-- 2155
SELECT COUNT(*) FROM [dbo].[Products] [pro], [dbo].[Order Details] [det]
	WHERE [pro].[ProductID] = [det].[ProductID]
```

This type of Join you wont use for practical application.


##EQUI JOIN
Like cross join but with where condition where field of table A has to be identical with a field of table B.

##NATURAL JOIN
To solve the problem of cross joins you can use a **WHERE** condition to join a field of the first table on a field of the second table.
Example:
```sql
-- 2155
SELECT COUNT(*) FROM [dbo].[Products] [pro], [dbo].[Order Details] [det]
	WHERE [pro].[ProductID] = [det].[ProductID]

-- Total Ordery for Countries
SELECT c.[Country], COUNT(DISTINCT o.[OrderID]) [TotalOrders],
COUNT(*) [TotalInvoices],
ROUND(SUM([Quantity] * [UnitPrice] * (1-[Discount])),2) AS [InvoiceTotal]
FROM [Customers] c, [Orders] o, [Order Details] od
WHERE c.[CustomerID] = o.[CustomerID] and o.[OrderID] = od.[OrderID] GROUP BY c.[Country] ORDER BY  [TotalOrders]

```

##INNER JOIN
Now the last query as an inner join
```sql
SELECT c.[Country], COUNT(DISTINCT o.[OrderID]) [TotalOrders],
COUNT(*) [TotalInvoices],
ROUND(SUM([Quantity] * [UnitPrice] * (1-[Discount])),2) AS [InvoiceTotal]
FROM [Customers] c
INNER JOIN [Orders] o ON c.[CustomerID] = o.[CustomerID]
INNER JOIN [Order Details] od ON o.[OrderID] = od.[OrderID]
GROUP BY c.[Country] ORDER BY  [TotalOrders]

-- 830
SELECT [c].[CustomerID], [c].[CompanyName], [o].[OrderID]
FROM [Customers] [c] INNER JOIN [Orders] [o] on [c].[CustomerID] = [o].[CustomerID]

```

##LEFT JOIN
In the last query we only get the customers witch already made an order. To get all customers we have to use a left join.
```sql
SELECT [c].[CustomerID], [c].[CompanyName], [o].[OrderID]
FROM [Customers] [c] LEFT JOIN [Orders] [o] on [c].[CustomerID] = [o].[CustomerID]
```
