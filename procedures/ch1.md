# SQL-Tutorial: Procedures
# Chapter 1: Northwind Examples I

##Procedur to check the stock

###Create a new Table
```sql
CREATE TABLE ProductStatus(
	ProductID INT NOT NULL PRIMARY KEY,
	OnStock INT NOT NULL,
	Requests INT NOT NULL
)
```

###Create the procedur
```sql
GO

CREATE PROCEDURE StockCheck
AS
BEGIN

	DECLARE @ProductID INT
	DECLARE @OnStock INT
	DECLARE @Requests INT

	DECLARE ProductCursor Cursor FOR
		SELECT [ProductID], [UnitsInStock] FROM [dbo].[Products]

	OPEN ProductCursor

	FETCH NEXT FROM ProductCursor INTO @ProductID, @OnStock

	WHILE @@FETCH_STATUS = 0
	BEGIN

		SET @Requests = (SELECT SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID)

		IF EXISTS (SELECT * FROM [ProductStatus] WHERE [ProductID] = @ProductID)
			UPDATE [dbo].[ProductStatus] SET OnStock = @OnStock, Requests = @Requests WHERE ProductID = @ProductID
		ELSE
			INSERT INTO [dbo].[ProductStatus] VALUES (@ProductID, @OnStock, @Requests)
		FETCH NEXT FROM ProductCursor INTO @ProductID, @OnStock
	END

	CLOSE ProductCursor
	DEALLOCATE ProductCursor
END

GO
```

###Execute the procedur
```sql
EXEC [dbo].[StockCheck]
```


###Optimized
Now we want a procedur that only checks one product.

```sql
GO

CREATE PROCEDURE StockCheckForOneProduct
@ProductID INT
AS
BEGIN

	DECLARE @OnStock INT
	DECLARE @Requests INT

	SET @OnStock = (SELECT [UnitsInStock] FROM [Products] WHERE [ProductID] = @ProductID)
	SELECT @Requests = SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID

	SET @Requests = (SELECT SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID)

	IF EXISTS (SELECT * FROM [ProductStatus] WHERE [ProductID] = @ProductID)
		UPDATE [dbo].[ProductStatus] SET OnStock = @OnStock, Requests = @Requests WHERE ProductID = @ProductID
	ELSE
		INSERT INTO [dbo].[ProductStatus] VALUES (@ProductID, @OnStock, @Requests)

END
GO
```

And finally we change the first procedure

```sql
ALTER PROCEDURE [dbo].[StockCheck]
AS
BEGIN

	DECLARE @ProductID INT
	DECLARE @OnStock INT
	DECLARE @Requests INT

	DECLARE ProductCursor Cursor FOR
		SELECT [ProductID], [UnitsInStock] FROM [dbo].[Products]

	OPEN ProductCursor

	FETCH NEXT FROM ProductCursor INTO @ProductID, @OnStock

	WHILE @@FETCH_STATUS = 0
	BEGIN
		EXEC [dbo].[StockCheckForOneProduct] @ProductID
		FETCH NEXT FROM ProductCursor INTO @ProductID, @OnStock
	END

	CLOSE ProductCursor
	DEALLOCATE ProductCursor
END

```


###Resume
We can use the following procedures
```sql
EXEC [dbo].[StockCheck]
EXEC [dbo].[StockCheckForOneProduct] 12
```

###Check
```sql
DELETE FROM [ProductStatus]
SELECT count(*) FROM [ProductStatus]

EXEC [dbo].[StockCheck]
SELECT count(*) FROM [ProductStatus]

DELETE FROM [ProductStatus] WHERE ProductID = 7
SELECT count(*) FROM [ProductStatus]

EXEC [dbo].[StockCheckForOneProduct] 7
SELECT count(*) FROM [ProductStatus]
```
