# CTE

## Enkele CTE

```sql
WITH CTE_Doelpunt AS (
    SELECT *
    FROM Doelpunt
    WHERE ScoreMinuten = 90
)

SELECT *
FROM CTE_Doelpunt
```

## CTE RECURSION

```sql
WITH numbers (number) AS (
    SELECT 1
    UNION ALL
    SELECT number + 1 
    FROM numbers 
    WHERE number < 999
)

SELECT * 
FROM numbers
OPTION (MAXRECURSION 1000);
```

## Traversing a hierarchical structure

```sql
WITH Bosses(boss, emp) AS 
( SELECT ReportsTo, EmployeeID
    FROM Employees
    WHERE ReportsTo IS NULL
    UNION ALL
    SELECT e.ReportsTo, e.EmployeeID
    FROM Employees e INNER JOIN Bosses b ON e.ReportsTo = b.emp
)

SELECT * FROM Bosses 
ORDER BY boss, emp;
```

## Drawing hierarchy structure

```sql
WITH Bosses(boss, emp, title, level, path) AS (
    -- Base case: Top-level employees with no boss
    SELECT ReportsTo, EmployeeID, Title, 1, CONVERT(varchar(max), Title)
    FROM Employees
    WHERE ReportsTo IS NULL
    UNION ALL
    -- Recursive case: Employees reporting to a boss
    SELECT e.ReportsTo, e.EmployeeID, e.Title, b.level + 1, 
           CONVERT(varchar(max), b.path + '<--' + e.Title)
    FROM Employees e INNER JOIN Bosses b ON e.ReportsTo = b.emp
)
-- Final query to get all results
SELECT * FROM Bosses
ORDER BY boss, emp;
```
