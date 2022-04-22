## SQL

Structured Query Language (SELECT, INSERT, DELETE, UPDATE)
Mit SQL lassen sich Einträge erstellen, bearbeiten, löschen oder ausgeben.
Es gibt viele erweiterte Funktionalitäten, mit welchen genauer gesucht oder gefiltert werden kann.

### SQL DDL

SQL Data Definition Language (CREATE, DROP)
Mit der data definition language legt man die Datenstruktur einer Datenbank fest.
Tasbellen und Datenbanken werden mit dem CREATE-Befehl erstellt und mit dem DROP-Befehl gelöscht.

### SQL DML

SQL Data Manipulation Language (ALTER)
Durch die data manipulation language können Änderungen an bestehenden Tabellen vorgenommen werden.


### Beispiele

DDL:
```sql
CREATE TABLE guest(
  id int,
  firstname text,
  lastname text,
  birthdate date,
  adress text,
  PRIMAREY KEY(id)
);
```

DML:
```sql
ALTER TABLE guest
DROP COLUMN adress;
```

SQL:
```sql
SELECT * FROM guest;
```

### Datentypen

Die SQL Datentypen lassen sich in folgende Kategorien einteilen:
<!-- tabs:start -->
#### **Numeric**

- INTEGER (INT)
- DECIMAL (DEC, FIXED) hohe Präzision
- NUMERIC hohe Präzision
- FLOAT (REAL)
- DOUBLE PRECISION (DOUBLE)
- BIT

#### **Date and Time**

- DATE
- TIME
- DATETIME kann auto-updaten
- TIMESTAMP kann auto-updaten
- YEAR

#### **String**

String-Typen können Zeichenfolgen abspeichern.

- CHAR
- VARCHAR
- BINARY Byte String
- VARBINARY BYte String
- BLOB Binary String
- TEXT
- ENUM Auswahl aus vordefinierten Strings
- SET kann keinen, einen oder mehrere vordefinierte Werte speichern

#### **Spatial**

Die räumlichen Datentypen werden allgemein für Geometriedaten verwendet.

- GEOMETRY
- POINT
- LINESTRING
- POLYGON
- MULTIPOINT
- MULTILINESTRING
- MULTIPOLYGON
- GEOMETRYCOLLECTION

#### **JSON**

Der JSON-Datentyp ermöglicht es direkt JSON in einem SQL-Feld zu speichern.
Gegenüber dem Speichern von JSON-Daten in einem normalen String-Feld, werden Daten in einem JSON-Feld validiert und optimiert gespeichert.

<!-- tabs:end -->

### Indexierung

Durchh einen Index werden die Einträge einer Spalte in einer weiteren Datenstruktur gespeichert, um bei einer Abfrage nach jener Spalte eine performantere Antwort zu erhalten. Indizes sind wichtig in grossen Tabellen (>10'000 Einträge) für Spalten die oft in WHERE- und ORDER BY-Statements verwendet werden.

Typen:

- Unique -> jeder Wert kann exakt 1 Mal vorkommen
- Primary Key -> wie Unique, es kann nur 1 PK pro Tabelle geben
- Index -> auch doppelte Werte sind erlaubt
- Fulltext -> in längeren Strings werden alle Wörter indiziert

MySQL Syntax: 
<Operation> [UNIQUE | FULLTEXT] INDEX <Index-Name>
ON <Tabellen-Name> (<Spalte>, <Spalte2>, ...)

Beispiel:
```sql
CREATE FULLTEXT INDEX index_paragraph
ON mytable (description);
```

## NoSQL


