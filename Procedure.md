# Procedure

```sql
CREATE/ALTER PROCEDURE ShowFirstXEmployees @x INT, @missed INT OUTPUT
AS 
DECLARE @empid INT, @fullname VARCHAR(100), @city NVARCHAR(30), @total int

SET @empid = 1
SELECT @total = COUNT(*) FROM Employees
SET @missed = 10

IF @x >@total
SELECT @x = COUNT(*) FROM Employees 
ELSE 
SET @missed = @total - @x

WHILE @empid <= @x
BEGIN
SELECT @fullname = firstname + ' ' + lastname, @city = city FROM Employees WHERE employeeid = @empid
PRINT 'Full Name : ' + @fullname
SET @empid = @empid + 1
END

-- Test de stored procedure
DECLARE @numberOfMissedEmployees INT
SET @numberOfMissedEmployees = 0
EXEC ShowFirstXEmployees 5, @numberOfMissedEmployees OUT
PRINT 'Number of missed employees: '  + STR(@numberOfMissedEmployees)

-- drop procedure
DROP PROCEDURE ShowFirstXEmployees
```
