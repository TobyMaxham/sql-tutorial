# SQL Tutorial: Functions
# Chapter 2: User defined Functions

```sql
GO
CREATE FUNCTION ProductRequests (@ProductID INT)
RETURNS INT
AS
BEGIN

	DECLARE @Summe INT
	SELECT @Summe = SUM([od].[Quantity]) FROM [dbo].[Order Details] [od] WHERE ProductID = @ProductID
	RETURN @Summe
END
GO

SELECT [dbo].[ProductRequests]([ProductID]), * FROM [dbo].[Products]
```



