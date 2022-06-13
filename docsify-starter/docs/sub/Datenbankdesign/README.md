# Datenbankdesign

## MongoDB

Dank der Flexibilität von MongoDB kann selbstständig entschieden werden, ob eine Datenbank mit Referencing oder Embedding aufgebaut wird.
Faktoren für die Entscheidung zwischen Referencing und Embedding sind die Datenmenge und die Möglichkeit Daten einzeln abrufen zu können.
Wird sich für das Referencing entschieden, ähnelt die Datenbankstruktur der eines RDBMS. Zwei Datensätze werden in einzelnen Collections verwaltet und per ID verknüpft.
Das Embedding von Datensätzen oder Werten ist vergleichbar mit dem JSON-Datentyp in MySQL. Es können Arrays oder gar ganze Datensätze in einem Datensatz erfasst werden. Dies nennt man auch Nesting von Datensätzen.

## Vor- und Nachteile

### Referencing

Vorteile:
* kleinere Documents
* weniger redundante Daten
* Daten einzel per Query verfügbar
  
Nachteile:
* kompliziertere Abfragen (Joins)

### Embedding

Vorteile:
* Alle Daten mit einer Query verfügbar
* weniger komplizierte Abfragen (keine Joins)
* CRUD-Operationen eines Documents sind ACID-kompatibel