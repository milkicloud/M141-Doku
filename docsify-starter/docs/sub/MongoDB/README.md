# MongoDB

MongoDB ist ein dokumentenorientiertes NoSQL-Datenbankmanagementsystem.
Es speichert Daten und Strukturen in JSON-ähnlichen Dokumenten (BSON - Binary JSON) ab.
MongoDB ist die weitest verbreitete NoSQL-Datenbank.

gut für unstrukturierte Daten

## Aufbauunterschiede

| RDBMS       | MongoDB                                         |
| ----------- | ----------------------------------------------- |
| Database    | Database                                        |
| Table       | Collection                                      |
| Tuple/Row   | Document                                        |
| column      | Filed                                           |
| Table Join  | Embedded Documents                              |
| Primary Key | Primary Key (von MongoDB automatisch generiert) |

## Installation

### Step by Step

1. Lizenz-Key importieren
   ```bash
   wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
   ```
   out:
   ![key-import](imgs/keyimport.png)
   1. Falls nicht erfolgreich gnupg installieren
   ```bash
    sudo apt install gnupg
    ```
   1. Lizenz-Key importieren
   ```bash
    wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -
    ```
2. Update-Sourcen erfassen
    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
    ```
    out:
    ![update-sourcen](imgs/updatesourcen-list.png)
3. Sourcen updaten
4. MongoDB installieren