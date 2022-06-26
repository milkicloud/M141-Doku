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

## MySQL




