## ACID

ACID (AKID) beschreibt erwünschte Eigenschaften von Datenverwaltungssoftware.

### Atomicity

Eine Änderung wird nur dann gespeichert, wenn der Änderungsprozess komplett durchgeführt werden konnte. 

### Consistency

Gewährleistet, dass die Integritätsbedingungen eingehalten werden, also die Einträge und Relationen korrekt gespeichert werden.

### Isolation

Wird eine Befehlsfolge auf dem System ausgeführt, wird gewährleistet, dass keine andere Befehlsfolge stören kann.

### Durability

Nach einer Befehlsfolge werden alle Daten dauerhaft auf der FEstplatte gespeichert.

## BASE

Häufig in NoSQL Datenbanken. Ein humoristischer Gegensatz zu ACID.
Man hält sich nicht an die Transaktionssicherheit, hat dafür aber zum Beispiel eine bessere Skalierbarkeit.

### Basically Available



### Soft State



### Eventually Consistent



## CAP-Theorem

### Consistency

Alle Datensätze sind überall identisch, werden also auf alle Server repliziert.

### Availability

Die Daten sind immer abrufbar.

### Partition tolerance

Datenbank kann auf mehrere Datenträger skaliert werden.

## Transactions

Eine Transaktion ist eine Befehlsfolge, welche nach dem ACID-prinzip ausgeführt wird.
Wird eine Transaktion ausgeführt, sind alle verwendeten Tabellen für andere Transaktionen kurzzeitig gesperrt.
Da dies die Performanz der Datenbank einschränk, gibt es Isolationsoptionen.

| Isolationsebene  | Dirty Read | Lost Updates | Non-Repeatable Query | Phantom Query |
| ---------------- | ---------- | ------------ | -------------------- | ------------- |
| Read Uncommitted | mölich     | möglich      | möglich              | möglich       |
| Read Committed   | unmöglich  | möglich      | möglich              | möglich       |
| Repeatable Read  | unmöglich  | unmöglich    | unmöglich            | möglich       |
| Serializable     | unmöglich  | unmöglich    | unmöglich            | unmöglich     |

