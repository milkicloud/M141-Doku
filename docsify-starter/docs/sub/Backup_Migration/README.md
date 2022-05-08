# MySQL

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
ssh user@SERVER1 'mysqldump --single-transaction -u root -p DB_NAME' 
    | ssh user@SERVER2 'mysql -u USER -p NEW_DB'
```


# MongoDB

## Backup

Mit dem Integrierten Programm "mongodump" kann ein Backup in Form von binären Daten erstellt werden. Das Backup erstellt Exportdateien im BSON-Format für die Datensätze und im JSON-Format für die Metadaten.

```bash
mongodump --host=mongodb1.example.net --port=27017 --username=user --authenticationDatabase=admin --out=opt/backup/dump-2022-05-04
```

## Restore

Um ein Backup wieder zu importieren kann "mongorestore" verwendet werden. 

```bash
mongorestore --uri="mongodb://user@mongodb1.example.net:27017/?authSource=admin" opt/backup/dump-2022-05-04
```