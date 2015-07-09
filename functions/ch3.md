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





###Returning a select
We make a function that will return us all employees that reports to a specific employee.
```sql
GO

CREATE FUNCTION [dbo].[getEmployeesReportsTo] (@EmployeeID INT)
RETURNS NVARCHAR(MAX)
AS
BEGIN

	DECLARE @Employee NVARCHAR(MAX)
	DECLARE @Result NVARCHAR(MAX) = ''

	DECLARE EmployeeCursor CURSOR FOR
	SELECT LastName FROM [dbo].[Employees] WHERE [ReportsTo] = @EmployeeID

	OPEN EmployeeCursor
	FETCH NEXT FROM EmployeeCursor INTO @Employee
	WHILE @@FETCH_STATUS = 0
	BEGIN
		IF @Result = ''
			SET @Result += @Employee
		ELSE
			SET @Result += ', ' + @Employee
		FETCH NEXT FROM EmployeeCursor INTO @Employee
	END

	CLOSE EmployeeCursor
	DEALLOCATE EmployeeCursor
	RETURN @Result

END

GO


SELECT [dbo].[getEmployeesReportsTo](2)
```
