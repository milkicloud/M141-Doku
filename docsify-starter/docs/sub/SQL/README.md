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
)
```


## NoSQL


