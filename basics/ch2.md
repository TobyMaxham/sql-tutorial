# PHP Tutorial: The Basics
# Chapter 2: CRUD


##Update
Simply uptdate the name of a specific product. In this case we want to update the name of the product with the ID *10*:
```sql
UPDATE [dbo].[ProductData] SET [ProductName] = 'Outback Black Lager' WHERE [ID] = 10
```

No we have to increase the unite price of all products we supply from Germany to 10%:
```sql
UPDATE [dbo].[Products] SET [UnitPrice] = [UnitPrice] * 1.1
	WHERE [SupplierID] IN (SELECT [SupplierID] FROM [dbo].[Suppliers] WHERE [Country] = 'Germany')
```


