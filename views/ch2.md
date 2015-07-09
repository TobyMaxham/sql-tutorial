# SQL Tutorial: Views
# Chapter 1: Trigger



##Before
We want to create a trigger that wont update or insert data if some validation has error.
Let us check how many orders an employee has worked on.
```sql
SELECT EmployeeID, COUNT(*) FROM [dbo].[Orders] GROUP BY EmployeeID
```
So, an employee should not has more than 130 orders. If we would insert a new order or change the employee we have to check if the number of orders for the employee would be more than 130.

```sql
GO

CREATE TRIGGER [dbo].[iuTrig_Orders]
ON [dbo].[Orders]
FOR INSERT, UPDATE
AS
BEGIN

	DECLARE @EmployeeID INT
	DECLARE @CurrentOrders INT

	DECLARE OrderCursor CURSOR FOR
	SELECT [EmployeeID] FROM INSERTED GROUP BY [EmployeeID]

	OPEN OrderCursor
	FETCH NEXT FROM OrderCursor INTO @EmployeeID

	WHILE @@FETCH_STATUS = 0
	BEGIN

		SELECT @CurrentOrders = COUNT(*) FROM [dbo].[Orders] WHERE [EmployeeID] = @EmployeeID

		IF (@CurrentOrders) > 130
		BEGIN
			ROLLBACK TRANSACTION
			RAISERROR ('The emploee with the id "%i" has to many orders', -1, -1, @EmployeeID)
			CLOSE OrderCursor
			DEALLOCATE OrderCursor
			RETURN
		END

		FETCH NEXT FROM OrderCursor INTO @EmployeeID
	END

	CLOSE OrderCursor
	DEALLOCATE OrderCursor
END

GO
```


##After
```sql
GO

CREATE TRIGGER dbo.OrderDetailsChangeTrigger
ON [dbo].[Order Details]
AFTER INSERT, UPDATE, DELETE
AS
BEGIN

	DECLARE @OrderID INT
	DECLARE @ProductID INT
	DECLARE @Status NVARCHAR(50)

	DECLARE ChangeCursor CURSOR FOR
	SELECT [OrderID], [ProductID] FROM INSERTED
	UNION
	SELECT [OrderID], [ProductID] FROM DELETED

	OPEN ChangeCursor
	FETCH NEXT FROM ChangeCursor INTO @OrderID, @ProductID

	WHILE @@FETCH_STATUS = 0
	BEGIN

		IF NOT EXISTS(SELECT * FROM INSERTED)
			SET @Status = 'DELETE'
		ELSE
		BEGIN
        IF NOT EXISTS(SELECT * FROM DELETED)
			SET @Status = 'INSERT'
        ELSE
			SET @Status = 'UPDATE'
		END

		INSERT INTO [dbo].[OrderDetailsChanges]#
		VALUES (@OrderID, @ProductID, @Status, CURRENT_TIMESTAMP, CURRENT_USER, SYSTEM_USER)


		FETCH NEXT FROM ChangeCursor INTO @OrderID, @ProductID
	END
	CLOSE ChangeCursor
	DEALLOCATE ChangeCursor

END

GO
```


##Instead of
In the next step we get the request, that an employee never should be deleted from the table.
First we create a new field called "DeletedAt".
```sql
ALTER TABLE [dbo].[Employees]
ADD DeletedAt datetime NULL
```

In the next step we define an instead of trigger that wont delete the data but will set the DeletedAt date.
```sql
GO
CREATE TRIGGER [dbo].[dTrig_Employees]
ON [dbo].[Employees]
INSTEAD OF DELETE
AS
BEGIN

	UPDATE [dbo].[Employees] SET [DeletedAt] = CURRENT_TIMESTAMP WHERE [EmployeeID] = (SELECT [EmployeeID] FROM DELETED)

END

GO
```

Now if we use the **DELETE** command, the data wont be deleted.
```sql
DELETE FROM [dbo].[Employees] WHERE [EmployeeID] = 7
```





