# SQL-Tutorial: Information Schema
#Some useage examples

##Show the meta data of a table
We want a function witch shows us a short version of the table info. We only want to the table name, the datatype and if a ***NULL*** value is allowed or not.
So we create the following function:
```sql
GO

CREATE FUNCTION [dbo].[getTableInfo] (@TableName NVARCHAR(MAX))
RETURNS @TableInfo TABLE (
	ColumnName NVARCHAR(MAX),
	ColumnType NVARCHAR(20),
	AllowsNull NVARCHAR(10)
)
AS
BEGIN

	INSERT INTO @TableInfo SELECT [COLUMN_NAME], [DATA_TYPE], [IS_NULLABLE] FROM [INFORMATION_SCHEMA].[COLUMNS] WHERE [TABLE_NAME] = @TableName

	RETURN
END

GO
```

As we say, that we return a "table" it is possible to use the function like a select on a real table:
```sql
SELECT * FROM [dbo].[getTableInfo]('Employees') ORDER BY [ColumnName]
```