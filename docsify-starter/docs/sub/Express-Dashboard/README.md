# Beispielapplikation - Express-Dashboard

## Installation

Git-Repository Klonen:
```bash
git clone https://github.com/sbhaseen/express-dashboard-demo.git
```

In Directory "express-dashboard-demo" wechseln und die npm Pakete instalieren:
```bash
npm install
```

Applikation starten:
```bash
npm run dev
```

Die Applikation läuft im developement Modus auf dem Port 5000.
Auf der Web-GUI können einträge erstellt und angesehen werden.

## DB-Benutzer hinzufügen und Berechtigen

Damit die Datenbank und das Datenbankcluster abgesichert sind, müssen User mit spezifischen Rollen erstellt werden.
Die Passwörter wurden jeweils nach dem Befehl per Prompt mitgegeben.

System Admin erfassen:
```mongo
use mfgdashboard
db.createUser({
    user: "sysAdmin",
    pwd: passwordPrompt(),
    roles:[
        { role: "dbAdminAnyDatabase", db: "Admin"},
    ]
})
```
pw: secure!SYS_adm1n


Applikationsbenutzer erfassen:
```mongo
use mfgdashboard
db.createUser({
    user: "appUser",
    pwd: passwordPrompt(),
    roles:[
        { role: "readWrite", db: "mfgdashboard"},
    ]
})
```
pw: th1s_appUSR

Damit die Applikation sich mit dem dafür definierten Benutzer anmeldet, wird der URI-Connection-String in der Applikation angepasst:
```js
const db = process.env.MONGODB_URI || 'mongodb://appUser:th1s_appUSR@localhost:27017/mfgdashboard';
```