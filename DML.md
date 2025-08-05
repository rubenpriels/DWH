# DML

```sql
BEGIN TRANSACTION;

INSERT INTO Doelpunt(ScoreMinuten, WieScoorde)
VALUES (102, 'U');

UPDATE Doelpunt
SET ScoreMinuten = 105
WHERE ScoreMinuten = 102

DELETE FROM Doelpunt
WHERE ScoreMinuten = 105

ROLLBACK;
```

```sql
-- MERGE
BEGIN TRANSACTION;

MERGE Shippers AS t -- t = target
USING ShippersUpdate AS s -- s = source
ON (t.ShipperID = s.ShipperID)

-- Update de rijen in de target (Shippers) als de waarden verschillen
WHEN MATCHED AND (t.CompanyName <> s.CompanyName OR ISNULL(t.Phone, '') <> ISNULL(s.Phone, ''))
THEN UPDATE SET t.CompanyName = s.CompanyName, t.Phone = s.Phone

-- Voeg nieuwe rijen toe aan de target (Shippers) die nog niet bestaan
WHEN NOT MATCHED BY TARGET -- Nieuwe rijen in de bron die niet in de target bestaan
THEN INSERT (CompanyName, Phone) VALUES (s.CompanyName, s.Phone)

-- Verwijder rijen uit de target (Shippers) die niet in de bron staan
WHEN NOT MATCHED BY SOURCE -- Rijen in de target die niet meer in de bron bestaan
THEN DELETE;

ROLLBACK;
```
