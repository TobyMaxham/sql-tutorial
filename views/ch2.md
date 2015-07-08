# SQL Tutorial: Views
# Chapter 1: Trigger



##Before
##After
##Instead of


### AFTER INSERT, UPDATE, DELETE
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