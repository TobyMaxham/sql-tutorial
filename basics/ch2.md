# SQL Tutorial: The Basics
# Chapter 2: Tables


```sql
CREATE TABLE [ProductData] (
	[ID] INT IDENTITY(1,1) NOT NULL,
	[ProductName] NVARCHAR(50) NOT NULL
)
```


```sql
CREATE TABLE OrderDetailsChanges (

	OrderID INT NOT NULL,
	ProductID INT NOT NULL,
	ChangedTime DATETIME,
	DBUser NVARCHAR(50),
	SysUser NVARCHAR(50)

	PRIMARY KEY (OrderID, ProductID, ChangedTime)
)
```