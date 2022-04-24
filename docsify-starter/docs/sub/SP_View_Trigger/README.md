## Stored Procedures

Stored Procedures sind auf der Datenbank gespeicherte SQL-Anweisungen.
Komplizierte SQL-Anweisungen können in Stored Procedures gespeichert werden, damit im Applikationscode keine langen Anweisungen abgebildet werden müssen. Ausserdem können ausschliesslich Berechtigungen für Stored Procedures festgelegt werden, damit eine Applikation zwar Stored Procedures ausführen kann, jedoch keine "eigene" Anweisungen.

### Syntax

```mysql
CREATE PROCEDURE sp_name ([proc_parameter[...]]) -- proc_parameter legt fest ob der Parameter ein In- oder Output ist
[characteristic ...] sp_body -- characteristic sind zusätzliche Optionen: DETERMINISTIC, SQL SECURITY, COMMENT
```

### Beispiel

Erstellen einer Stored Procedure:
```mysql
delimiter // -- vorübergehend umstellen, damit ";" in die SP geschrieben werden kann
CREATE PROCEDURE mysp (OUT param TEXT)
BEGIN
SELECT description INTO param FROM table; -- SQL-Anweisung
END;
// -- ausführen um SP zu erstellen
delimiter ; -- delimiter wieder umstellen
```

Aufrufen einer Stored Procedure:
```mysql
CALL mysp(@out); -- Aufrufen der SP und speichern des Outputs in die globale Variable @out
SELECT @out; -- Ausgeben des Outputs
```

## Views

Im Grunde sind Views wie Stored Procedures, nur beschränkt auf SELECT-Anweisungen.
Es handelt sich meist um SELECT-Anweisungen mit einem oder mehreren JOINs.

### Syntax

```mysql
CREATE [OR REPLACE] VIEW v_name [(column_list)] AS view_body 
-- view_body enthält das zu speichernde SELECT-Statement
-- in der column_list werden alternative Namen für die Spalten der View definiert
```

### Beispiel

```mysql
CREATE VIEW somejoin (Name, Einkaufsdatum) AS
    SELECT p.name, e.date FROM person AS p
    JOIN einkauf AS e on p.id = e.p_id;
```


## Triggers

Für einen Trigger muss ein Ereignis definiert werden. Tritt dieses Ereignis ein, führt der Trigger eine gewünschte Anweisung aus.
Im Grunde ist es eine Stored Procedure, welche automatisch bei Eintreffen eines festgelegten Events ausgeführt wird.

### Syntax

```mysql
CREATE TRIGGER trigger_name 
trigger_time trigger_event -- trigger_time: {BEFORE | AFTER} trigger_event: {INSERT | UPDATE | DELETE}
ON table_name FOR EACH ROW [trigger_order] -- trigger_order: {FOLLOWS | PRECEDES} other_trigger_name
trigger_body -- trigger_body ist die auszuführende SQL-Anweisung
```

### Beispiel

```mysql
CREATE TRIGGER new_user 
AFTER INSERT ON user
FOR EACH ROW
INSERT INTO object VALUES("user", NEW.id);
```
