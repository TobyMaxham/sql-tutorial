# SQL Tutorial: The Basics
# Chapter 5: Irrelevant & Fun

```sql
SELECT SPACE((SELECT MAX(LEN([LastName])) FROM [dbo].[Employees]) - LEN([LastName])) + [LastName] FROM [dbo].[Employees]
```