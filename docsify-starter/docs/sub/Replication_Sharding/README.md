# Replication und Sharding

Ein Replication-Cluster besteht aus mehreren Nodes, welche sich unter einander abgleichen.

Ein Sharding-Cluster teilt die gesamte Datenbank auf mehrere Nodes auf.

Beide Methoden ergeben ein Load-Balancing. Ob sich ein Sharding- oder ein Replication-Cluster mehr lohnt ist abhängig vom Aufbau der 

## Mongo

Als erstes werden zwei Mongo-Instanzen mit dem Parameter replSet gestartet.

```bash
mongod --replSet book --dbpath ./mongo1 --port 27011
```

```bash
mongod --replSet book --dbpath ./mongo2 --port 27012
```

Um den Replication-Cluster zu starten, loggt man sich auf einer der Instanzen ein und führt den folgenden Befehl aus:
```mongo
rs.initiate({
    _id: 'book',
    members: [
        {_id: 1, host: 'localhost:27011'},
        {_id: 2, host: 'localhost:27012'}
    ]
})
```

Ein Connectionstring auf das Replication-Cluster würde wie folgt aussehen:
"mongodb://localhost:27011,localhost:27012/replicaSet=book"

Um ein Sharding-Cluster zu erzeugen muss beim starten der beiden Mongo-Instanzen zusätzlich der Parameter "--shardsvr" hinzugefügt werden.

## MySQL

Das Clustering unter einem RDBMS ist grundsätzlich komplexer. Zusätzlich zu allen Daten, welche repliziert werden, muss auch die Transaktionssicherheit nach wie vor gewährleistet sein. Für die Replikation werden Binary-Logs, welche alle Transaktionen beinhalten, ausgetauscht.

Das Routing der Connections und Queries übernimmt bei MySQL ein Proxy-Dienst. Beispiele dafür wären ProxySQL, MariaDB MaxScale oder MySQL-Router.

### Active/Passive

Alle Reads und Writes werden auf der selben Instanz ausgeführt und die Änderungen werden auf die Replicas repliziert.

### Active/Read Pool

Alle Writes werden auf der Source ausgeführt und alle Reads werden auf den Replicas ausgeführt.

### Cluster erstellen

1. Konfiguration des Masters anpassen
´´´bash
[mysqld]
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
datadir = /var/lib/mysql
log-error = /var/log/mysql/error.log
server-id = 1 # Wir brauchen eine ID
log-bin = /var/log/mysql/mysql-bin.log # Wir brauchen das BIN-LOG
tmpdir = /tmp
binlog_format = ROW # Wir machen ROW-Based
max_binlog_size = 800M
sync_binlog = 1
expire-logs-days = 5
slow_query_log=1
slow_query_log_file=/var/lib/mysql/mysqld-slow.log
´´´

2. Dienst neustarten
3. Benutzer für die Replikation erstellen
´´´mysql
CREATE USER replication_user@192.168.178.137 IDENTIFIED BY 'StrongP@ssw0rd';
´´´

4. Konfiguration des Slave-Nodes anpassen
´´´bash
[mysqld]
log_bin = /var/log/mysql/mysql-bin.log
server-id = 2 # Wir brauchen eine ID
read_only = 1 # Wir sind Slave
tmpdir = /tmp 
binlog_format = ROW # Wir machen ROW-Based
max_binlog_size = 800M
sync_binlog = 1
expire-logs-days = 5
slow_query_log = 2
´´´
5. Position und Logfile des Master Servers auslesen und merken
```mysql
SHOW MASTER STATUS\G
```

5. Dienst neustarten
6. Verbindung von Slave zu Master herstellen
MASTER_LOG_FILE = Schritt 5 ausgelesenes Logfile
MASTER_LOG_POS = Schritt 5 ausgelesene Position
```mysql
CHANGE MASTER TO MASTER_HOST='192.168.12.12', MASTER_USER='replication_user', MASTER_PASSWORD='StrongP@ssw0rd', MASTER_LOG_FILE='mysql-bin.000002', MASTER_LOG_POS=156;
```
8. Proxy einrichten
