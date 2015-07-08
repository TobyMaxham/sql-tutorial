# SQL-Tutorial: Procedures
# Chapter 1: Northwind Examples II

##Procedur with returning select

```sql
GO
CREATE PROCEDURE Requests1
@ProductID INT
AS
	SELECT SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID
GO
```

###Using
And we could call the procedure.

```sql
EXEC Requests1 12
```



##Procedur with Output

```sql
GO
CREATE PROCEDURE Requests2
@ProductID INT,
@Summe INT OUTPUT
AS
	SELECT @Summe = SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID
GO

DECLARE @Summe INT
EXEC Requests2 12, @Summe OUTPUT
SELECT @Summe
```


##Procedur with returning value
```sql
GO
CREATE PROCEDURE Requests3
@ProductID INT
AS
	DECLARE @Summe INT

	SELECT @Summe = SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID
	RETURN @Summe
GO

DECLARE @Summe INT
EXEC @Summe = Requests3 12
SELECT @Summe
```
