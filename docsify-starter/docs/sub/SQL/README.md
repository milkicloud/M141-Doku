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

### Export Dump
  
Ein SQL-Dump erstellt ein SQL-Skript aus einer bestehenden Datenbank.
Das Skript ist ein genaues Abbild der Datenbank zum Zeitpunkt des Dumps.
Unter MySQL kann dafür der Befehl "mysqldump" genutzt werden.

Syntax:  
```bash
mysqldump [options] [DB_Name] [Table_Name, Table2_Name, ...] > File_Name.sql
```
  
Beispiele:
  
Es soll eine bestimmte Tabelle exportiert werden:
```bash
mysqldump mydb table1 > dump.sql
```
  
Es sollen bestimmte Datenbanken exportiert werden:
```bash
mysqldump --databases mydb, hisdb > dump.sql
```
  
Es sollen alle Datenbanken exportiert werden:
```bash
mysqldump --all-databases > dump.sql
```
  
### Import Dump
  
Um einen SQL-Script mit MySQL einzulesen wird der Befehl "mysql" verwendet.
Ein SQL-Dump kann so als Skript-File eingelesen werden.
 
Es sollen bestimmte Datenbanken exportiert werden:
```bash
mysql -u root -pPASSWORT [DB_Name] < dump.sql
```
 
### Export CSV
  
Beim Export in eine CSV-Datei wird das Ergebnis einer SELECT-Query in eine CSV-Datei geschrieben.
Das kann direkt mit SQL gemacht werden:            
```mysql
SELECT * FROM table1 WHERE age = 20 -- irgendeine SELECT-Query
INTO OUTFILE '/tmp/export.csv' -- Pfad der Ausgabe
FIELDS ENCLOSED BY '"' -- Felder werden mit "" eingefasst
TERMINATED BY ';' -- Felder werden mit ; abgeschlossen
LINES TERMINATED BY '\n' -- Zeilenumbruch nach jedem Eintrag                                           
```

...oder auch mit mysqldump,
für eine ganze Datenbank:                                             
```bash
mysqldump DB_Name
--fields-terminated-by ';' \
--fields-enclosed-by '"' \
--tab /tmp/export/
```                                             
   
eine einzelne Tabelle:     
```bash
mysqldump DB_Name Table_Name
--fields-terminated-by ';' \
--fields-enclosed-by '"' \
--tab /tmp/export/
```                                         

### Import CSV

Um eine CSV-Datei wieder zu importieren kann man wieder SQL nutzen:
```mysql
LOAD DATA INFILE '/tmp/export.csv'
INTO TABLE table1
FIELD TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS -- falls die Spalten-Header exportiert wurden, die 1. Zeile ignorieren                                     
```
                                             
...oder die MySQL-Utility "mysqlimport"
```bash
mysqlimport
--ignore-lines=1 \ # um allfälliger Header zu ignorieren
--fields-terminated-by=; \
--local -u root -p DB_name # Verbindung zur Datenbank
/tmp/export.csv # zu importierendes File
```

## NoSQL


