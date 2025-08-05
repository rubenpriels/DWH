# Trigger

- [Trigger](#trigger)
  - [Insert trigger](#insert-trigger)
  - [Update trigger](#update-trigger)
  - [Update if](#update-if)
  - [Insert en Update trigger](#insert-en-update-trigger)

## Insert trigger

```sql
CREATE OR ALTER TRIGGER insertOrderDetails ON OrderDetails FOR INSERT
AS
-- Declare variabelen om de ingevoegde waarden op te slaan
DECLARE @insertedProductID INT = (SELECT ProductID FROM inserted)
DECLARE @insertedUnitPrice Money = (SELECT UnitPrice FROM inserted)
DECLARE @unitPriceFromProducts Money = (SELECT UnitPrice FROM Products WHERE ProductID = @insertedProductID)

IF @insertedUnitPrice NOT BETWEEN @unitPriceFromProducts * 0.85 AND @unitPriceFromProducts * 1.15
BEGIN
       ROLLBACK TRANSACTION
       RAISERROR ('The inserted unit price can''t be correct', 14, 1)
END
```

## Update trigger

```sql
CREATE OR ALTER TRIGGER updateOrderDetails ON OrderDetails FOR update
AS
DECLARE @updatedProductID INT = (SELECT ProductID FROM inserted)
DECLARE @updatedUnitPrice MONEY = (SELECT UnitPrice FROM inserted)
DECLARE @unitPriceFromProducts MONEY = (SELECT UnitPrice FROM Products WHERE ProductID = @updatedProductID)

IF @updatedUnitPrice NOT BETWEEN @unitPriceFromProducts * 0.85 AND @unitPriceFromProducts * 1.15
BEGIN
       ROLLBACK TRANSACTION
       RAISERROR ('The updated unit price can''t be correct', 14, 1)
END
```

## Update if

```sql
CREATE OR ALTER TRIGGER updateOrderDetails ON OrderDetails FOR update
AS
-- Controleer of de kolom UnitPrice is bijgewerkt
IF UPDATE(unitPrice)
BEGIN
DECLARE @updatedProductID INT = (SELECT ProductID FROM inserted)
DECLARE @updatedUnitPrice MONEY = (SELECT UnitPrice FROM inserted)
DECLARE @unitPriceFromProducts MONEY = (SELECT UnitPrice FROM Products WHERE ProductID = @updatedProductID)

IF @updatedUnitPrice NOT BETWEEN @unitPriceFromProducts * 0.85 AND @unitPriceFromProducts * 1.15
BEGIN
      ROLLBACK TRANSACTION
      RAISERROR ('The updated unit price can''t be correct', 14, 1)
END
END

-- Controleer of de kolom Discount is bijgewerkt
IF UPDATE(Discount)
BEGIN
DECLARE @updatedDiscount REAL = (SELECT Discount FROM inserted)

-- Controleer of de korting tussen 0 en 0.25 ligt
IF @updatedDiscount NOT BETWEEN 0 AND 0.25
BEGIN
      ROLLBACK TRANSACTION
      RAISERROR ('The updated discount can''t be correct', 14, 1)
END
END
```

## Insert en Update trigger

```sql
CREATE OR ALTER TRIGGER updateOrInsertOrderDetails ON OrderDetails FOR update, insert
AS
DECLARE @updatedProductID INT = (SELECT ProductID FROM inserted)
DECLARE @updatedUnitPrice MONEY = (SELECT UnitPrice FROM inserted)
DECLARE @unitPriceFromProducts MONEY = (SELECT UnitPrice FROM Products WHERE ProductID = @updatedProductID)

IF @updatedUnitPrice NOT BETWEEN @unitPriceFromProducts * 0.85 AND @unitPriceFromProducts * 1.15
BEGIN
      ROLLBACK TRANSACTION
      RAISERROR ('The unit price can''t be correct', 14, 1)
END
```
