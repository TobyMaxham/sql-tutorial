# SQL-Tutorial: Procedures
# Chapter 1: Northwind Examples II

##Procedur with returning select


```sql
GO
CREATE PROCEDURE Requests
@ProductID INT
AS
	SELECT SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID
GO
```

###Using
And we could call the procedure.

```sql
EXEC Requests 12
```

