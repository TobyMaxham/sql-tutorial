# SQL Tutorial: The Basics
# Chapter 2: Selects




##LIKE
Now let us find all City witch begin with a "M":
```sql
SELECT [City] FROM [dbo].[Customers] WHERE [City] LIKE 'M%'
```
**Remember:** You also could use 'm%'. It depends on your server settings.


```sql
-- Only one result
SELECT [ProductName] FROM [dbo].[ProductData] WHERE [ProductName] LIKE 'Wasser\_%' ESCAPE '\'

-- Two results
SELECT [ProductName] FROM [dbo].[ProductData] WHERE [ProductName] LIKE 'Wasser_%'
```



##BETWEEN
```sql
SELECT * FROM [dbo].[Customers] WHERE [CustomerID] BETWEEN 'C' AND 'D'
```

##GROUP BY
```sql
SELECT [City], COUNT(*) FROM [dbo].[Customers] GROUP BY [City]
```

```sql
SELECT [City], COUNT(*) AS NumberOfCustomers FROM [dbo].[Customers] GROUP BY [City] HAVING COUNT(*) > 4 ORDER BY NumberOfCustomers
```

This SQL tells us, in wich countries we have more than 10 customers:
```sql
SELECT [Country], [Region], COUNT(*) AS [NumberOfCustomers] FROM [dbo].[Customers] GROUP BY [Country], [Region] HAVING COUNT(*) > 10 ORDER BY [Country], [Region]
```


##LAG
```sql
SELECT [CompanyName],
	LAG([CompanyName], 1,0) OVER (ORDER BY [CompanyName]) AS PreviousCompany,
	COUNT(*)
FROM [dbo].[Customers]
GROUP BY [CompanyName] ORDER BY [CompanyName
```


##Aggregate Functions
Now we have learned some default select statements. Further we can use **Aggregate Functions** to perform a calculation on a set of values.
I show you a few statemates for each aggregate functions.

###SUM
```sql
SELECT [OrderID], ROUND(SUM([Quantity] * [UnitPrice] * (1-[Discount])),2) AS [InvoiceTotal] FROM [dbo].[Order Details] GROUP BY [OrderID] ORDER BY [InvoiceTotal] DESC
```

###AVG
```sql
SELECT AVG([UnitPrice]) FROM [dbo].[Products]
```


