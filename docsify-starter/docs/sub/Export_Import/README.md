# MySQL

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

# MongoDB

## Export JSON

Das integrierte Tool "mongoexport" ermöglicht den Export von MongoDB-Daten in JSON- oder CSV-Dateien.
Für den Export in ein JSON-File muss der Connection-String, in welchem auch der Name der Datenbank vorhanden sein sollte, die Collection und die Ausgabedatei angegeben werden.
```bash
mongoexport --uri="mongodb://mongodb0.example.com:27017/reporting"  --collection=events  --out=events.json [additional options]
```

Mit dem Parameter "-q" kann eine Query für den Export mitgegeben werden.
```bash
mongoexport --collection=events --db=reporting -q='{ "a": { "$gte": 3 }, "date": { "$lt": { "$date": "2016-01-01T00:00:00.000Z" } } }' --out=myRecords.json
```

## Export CSV

Für einen Export in eine CSV-Datei muss zusätzlich der Parameter "--type=csv" angegeben werden. Ansonsten ist es das selbe, wie beim Export in eine JSON-Datei.

## Import JSON

Mit dem integrierten Programm "mongoimport" können JSON-Files in eine Datenbank importiert werden. Dazu wird die Angabe eines Connection-Strings, einer Datenbank, einer Collection und eines Files, welches die zu importierenden Daten beinhaltet, benötigt.
```bash
mongoimport --uri="mongodb://mongodb0.example.com:27017/reporting"  --db=users --collection=contacts --file=contacts.json
```

Falls bereits Daten in der angegebenen Collection bestehen, möchte man definieren, was mit duplikaten (Einträge mit selber _id) passiert.

soll ein bereits existierendes Document mit selber _id...
...ersetzt werden:
```bash
--mode=upsert
```

...gemergt werden:
```bash
--mode=merge
```

...gelöscht werden:
```bash
--mode=delete
```

## Import CSV

Soll eine CSV-Datei importiert werden, muss dem Befehl durch den Parameter "--type" mitgeteilt werden, dass es sich um eine CSV-Datei handelt. Ausserdem sollte der Parameter "--headerline" gesetzt werden, da man die Kopfzeile nicht als Eintrag speichern will.
```bash
mongoimport --uri="mongodb://mongodb0.example.com:27017/reporting" --db=users --collection=contacts --type=csv --headerline --file=/opt/backups/contacts.csv
```

Es können auch Types für die jeweiligen Felder mitgegeben werden:
```bash
mongoimport --db=users --collection=contacts --type=csv \
   --columnsHaveTypes \
   --fields="name.string(),birthdate.date(2006-01-02),contacted.boolean(),followerCount.int32(),thumbnail.binary(base64)" \ # Hier werden jetzt die einzelnen Spalten speziell behandelt
   --file=/example/file.csv
```