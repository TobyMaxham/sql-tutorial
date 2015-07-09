# SQL-Tutorial: Special
# Chapter 1: Check


##Check
We have to alter our table first and add the Check.
```sql
ALTER TABLE [dbo].[Orders]
	WITH NOCHECK ADD CONSTRAINT [CK_Orders] CHECK (
		(datediff(day, [ShippedDate], [RequiredDate]) > (2) )
	)
```

No try to insert some new data:
```sql
--- This Query don't works
INSERT INTO [dbo].[Orders]
(CustomerID, EmployeeID, OrderDate, RequiredDate, ShippedDate)
VALUES
('VINET', 5, CURRENT_TIMESTAMP, DATEADD(DAY, 3, GETDATE()), DATEADD(DAY, 5, GETDATE()))

--- This Query works
INSERT INTO [dbo].[Orders]
(CustomerID, EmployeeID, OrderDate, RequiredDate, ShippedDate)
VALUES
('VINET', 5, CURRENT_TIMESTAMP, DATEADD(DAY, 3, GETDATE()), DATEADD(DAY, 0, GETDATE()))
```