# Cursor

## 1 Cursor

```sql
DECLARE @supplierID INT, @companyName NVARCHAR(30), @city NVARCHAR(15)

DECLARE suppliers_cursor CURSOR FOR
    SELECT SupplierID, CompanyName, City
    FROM Suppliers
    WHERE Country = 'USA'

OPEN suppliers_cursor

-- Haal de eerste rij op
FETCH NEXT FROM suppliers_cursor INTO @supplierID, @companyName, @city

-- Loop door de resultaten totdat alle gegevens zijn opgehaald
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Print de gegevens van de supplier
    PRINT 'Supplier: ' + STR(@supplierID) + ' ' + @companyName + ' ' + @city
    -- Haal de volgende rij op
    FETCH NEXT FROM suppliers_cursor INTO @supplierID, @companyName, @city
END

CLOSE suppliers_cursor
DEALLOCATE suppliers_cursor
```

## Nested Cursor

```sql
DECLARE @supplierID INT, @companyName NVARCHAR(30), @city NVARCHAR(15), @productID INT, @productName NVARCHAR(40), @unitPrice MONEY

-- Declareer de cursor voor leveranciers
DECLARE suppliers_cursor CURSOR FOR
    SELECT SupplierID, CompanyName, City
    FROM Suppliers
    WHERE Country = 'USA'

-- Open de leverancierscursor
OPEN suppliers_cursor

-- Haal de eerste rij op van de leverancierscursor
FETCH NEXT FROM suppliers_cursor INTO @supplierID, @companyName, @city

-- Loop door alle leveranciers totdat er geen rijen meer zijn
WHILE @@FETCH_STATUS = 0
BEGIN
    -- Print de gegevens van de leverancier
    PRINT 'Supplier: ' + STR(@supplierID) + ' ' + @companyName + ' ' + @city

    DECLARE products_cursor CURSOR FOR
        SELECT ProductID, ProductName, UnitPrice
        FROM Products
        WHERE SupplierID = @supplierID

    OPEN products_cursor

    -- Haal de eerste rij op van de productencursor
    FETCH NEXT FROM products_cursor INTO @productID, @productName, @unitPrice

    -- Loop door alle producten van deze leverancier
    WHILE @@FETCH_STATUS = 0
    BEGIN
        -- Print de gegevens van het product
        PRINT ' - ' + STR(@productID) + ' ' + @productName + ' ' + STR(@unitPrice) + ' EUR'

        -- Haal de volgende rij op van de productencursor
        FETCH NEXT FROM products_cursor INTO @productID, @productName, @unitPrice
    END

    CLOSE products_cursor
    DEALLOCATE products_cursor

    -- Haal de volgende rij op van de leverancierscursor
    FETCH NEXT FROM suppliers_cursor INTO @supplierID, @companyName, @city
END

CLOSE suppliers_cursor
DEALLOCATE suppliers_cursor
```

## Delete Cursor

```sql
DECLARE @shipperID INT, @companyName NVARCHAR(100)

-- Declareer de cursor voor de shippers
DECLARE shippers_cursor CURSOR FOR
    SELECT ShipperID, CompanyName FROM Shippers

OPEN shippers_cursor
FETCH NEXT FROM shippers_cursor INTO @shipperID, @companyName

-- Loop door alle shippers totdat er geen rijen meer zijn
WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT '-' + STR(@shipperID) + ' ' + @companyName

    -- Als de shipperID groter is dan 4, verwijder de shipper
    IF @shipperID > 4
    -- BEGIN
        DELETE FROM Shippers WHERE CURRENT OF shippers_cursor
    -- END
    FETCH NEXT FROM shippers_cursor INTO @shipperID, @companyName
END

CLOSE shippers_cursor
DEALLOCATE shippers_cursor
```
