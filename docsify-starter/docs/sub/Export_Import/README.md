## Export Dump
  
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
  
## Import Dump
  
Um einen SQL-Script mit MySQL einzulesen wird der Befehl "mysql" verwendet.
Ein SQL-Dump kann so als Skript-File eingelesen werden.
 
Es sollen bestimmte Datenbanken exportiert werden:
```bash
mysql -u root -p [DB_Name] < dump.sql
```
 
## Export CSV
  
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

## Import CSV

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
 