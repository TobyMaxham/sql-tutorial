# SQL Tutorial: Functions
# Chapter 1: Date

```sql
-- The day after tomorrow
SELECT DATEADD(DAY, 2, GETDATE())

-- Next year
SELECT DATEADD(YEAR, 1, GETDATE())

-- 48 Hours
SELECT DATEDIFF(HOUR, '2015-07-11', '2015-07-13')

-- July
SELECT DATENAME(MONTH, '2015-07-11')

-- 7
SELECT DATEPART(MONTH, '2015-07-11')
```