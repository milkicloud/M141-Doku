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
mysql -u root -p [DB_Name] < dump.sql
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

## Backup

Im Falle eines Angriffes oder eines Fehlers ist es wichtig eine Sicherheitskopie seiner Datenbank zu haben.
                                             
### Begriffe

- **Cold:** DB steht während Backup **nicht** zur Verfügung
- **Warm:** DB steht während Backup **beschränkt** zur Verfügung (z.B. read-only)
- **Hot:** DB steht während Backup **vollständig** zur Verfügung
- **logisch:** Backup ist Human-readable
- **physisch:** Backup in binärer Form
- **full:** komplettes Backup
- **incremental:** Backup der Differenz zum letzten Backup

### Backup Beispiele

#### mysqldump - warm, logisch, full
Backup:
```bash
mysqldump --single-transaction -u root -p --all-databases > dump.sql 
# --single-transaction: fasst die Operation in eine SQL Transaction ein (BEGIN TRAN; COMMIT TRAN;)
# --all-databases: Backup aller Datenbanken auf dem Server
```
Restore:
```bash
mysql -u root -p < dump.sql
```

#### Copyjob - cold, physisch, full/inkrementell

```bash
rsync -avz /var/lib/mysql/* /IRGENDEINANDERSVERZEICHNIS/ 
# oder primitver:
cp -ar /var/lib/mysql/* /IRGENDEINANDERSVERZEICHNIS/
```

#### Backuptool Xtrabackup

Xtrabackup ist ein externes Tool zur Verwaltung von MySQL-Backups und Restores.
Es bietet viele diverse Backup Optionen und ist gratis verfügbar.

Full Backup:
```bash
xtrabackup --backup --target-dir=/data/backups/base
```

Incremental Backup:
Vor dem inkrementellen Backup muss immer zu erst ein Full Backup erstellt werden.
Bei Inkrementellen Backups werden .delta-Files gespeichert, welche die Differenzen darstellen.
1. Incremental Backup:
```bash
xtrabackup --backup --target-dir=/data/backups/inc1 \
--incremental-basedir=/data/backups/base
```

Das 2. inkrementelle Backup nutzt dann das 1. als basedir.

2. Incremental Backup:
```bash
xtrabackup --backup --target-dir=/data/backups/inc2 \
--incremental-basedir=/data/backups/inc1
```

Restore:
```bash
xtrabackup --copy-back --target-dir=/data/backups/
```

## Migration

Grundsätzlich kann für eine einfache Migration ein SQL-Dump der einen Datenbank auf einer zweiten Datenbank ausgeführt werden. Mit "mysqldump" wird ein Dump vom Ausgangssystem erzeugt und dieser wird per "mysql" auf dem Zielsystem importiert.
Im folgenden Beispiel muss bereits vor der Migration die Datenbank auf dem Zielsystem bestehen, dies kann auch durch die Optionen "--databases" und "--add-drop-databases" gelöst werden. Ausserdem  muss eine TCP-Verbindung möglich sein und der Verwendete Benutzer muss für den externen Zugriff berechtigt sein.

```bash
# Export - lokal zu remote
mysqldump --single-transaction -u user -p dbname -h localhost \
    | mysql -u root -p -h 192.168.1.1 dbname

# Import - remote zu lokal
mysqldump --single-transaction -u user -p dbname -h 192.168.1.1 \ 
    | mysql -u root -p -h localhost dbname
```

Es ist auch möglich eine Migration über eine SSH-Verbindung durchzuführen.
Auch dabei wird eine Pipe verwendet um den Dump direkt zu importieren.
```bash
ssh user@SERVER1 'mysqldump --single-transaction -u root -pPASS DBNAME' 
    | ssh user@SERVER2 'mysql -u USER -pPASS NEWDB'
```
                    
## NoSQL


