# Window

- [Window](#window)
  - [Range](#range)
  - [Window operations](#window-operations)

## Range

```sql
SELECT CategoryID, ProductID, UnitsInStock,
SUM(UnitsInStock) OVER (PARTITION BY CategoryID ORDER BY ProductID RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as 'TotalUnitsInStockPerCategory'
FROM Products
```

## Window operations

```sql
SELECT EmployeeID, FirstName + ' ' + LastName As 'Full Name', Title, Salary,
ROW_NUMBER() OVER (PARTITION BY Title ORDER BY Salary DESC) As 'ROW_NUMBER',
RANK() OVER (PARTITION BY Title ORDER BY Salary DESC) AS 'RANK',
DENSE_RANK() OVER (PARTITION BY Title ORDER BY Salary DESC) AS 'DENSE_RANK',
PERCENT_RANK() OVER (PARTITION BY Title ORDER BY Salary DESC) AS 'PERCENT_RANK',
SUM(Salary) OVER (ORDER BY Salary DESC ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS 'sum',
LEAD(Salary, 2) OVER (ORDER BY Salary DESC) as 'Salary',
LAG(Salary, 2) OVER (ORDER BY Salary DESC) as 'Salary'
FROM Employees
```
