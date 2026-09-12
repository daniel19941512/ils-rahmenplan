# Rahmenplan-Werkstatt mit Firebase – Einrichtung

Kein Google-Login für Nutzer nötig. Jeder mit dem Link kann speichern und öffnen.
Wichtig: das bedeutet auch, dass niemand extra freigeschaltet werden muss - aber
auch, dass jeder mit dem Link Entwürfe ändern oder löschen könnte. Für ein internes
Team in der Regel ein akzeptabler Kompromiss.

---

## Teil 1: Firebase-Projekt anlegen (ca. 5 Minuten, kein Kreditkarte nötig)

1. Gehe zu **https://console.firebase.google.com/**
2. **"Projekt hinzufügen"** → Namen eingeben (z.B. "ils-rahmenplan") → Google Analytics
   kann deaktiviert werden (nicht nötig) → Projekt erstellen
3. Im linken Menü: **"Build" → "Firestore Database"** → **"Datenbank erstellen"**
   - Standort: eine Region in Europa wählen (z.B. `eur3` / Frankfurt)
   - Modus: **"Testmodus"** auswählen (wir passen die Regeln gleich manuell an, s. Teil 2)
4. Im linken Menü: **Projekteinstellungen (Zahnrad-Symbol oben) → "Allgemein"**
   - Nach unten scrollen zu **"Ihre Apps"** → **"</>"** (Web-App) klicken
   - Namen vergeben (z.B. "ils-rahmenplan-web") → **"App registrieren"**
   - Es erscheint ein Codeblock mit `firebaseConfig = {...}` → diese 6 Werte
     (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId) kopieren

---

## Teil 2: Zugriffsregeln setzen (wichtig!)

Im linken Menü: **"Build" → "Firestore Database" → "Regeln"** (Tab oben)

Den Inhalt ersetzen durch:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

→ **"Veröffentlichen"** klicken.

Das macht die Datenbank dauerhaft offen zugänglich (kein automatisches Ablaufdatum
wie im Testmodus-Standard, der nach 30 Tagen sperrt). Ohne diesen Schritt würde die
App nach 30 Tagen aufhören zu funktionieren.

---

## Teil 3: Werte in die App eintragen

In `index.html` diese Zeilen suchen und mit den Werten aus Teil 1 füllen:

```js
const FIREBASE_CONFIG = {
  apiKey: "HIER_EINTRAGEN",
  authDomain: "HIER_EINTRAGEN.firebaseapp.com",
  projectId: "HIER_EINTRAGEN",
  storageBucket: "HIER_EINTRAGEN.appspot.com",
  messagingSenderId: "HIER_EINTRAGEN",
  appId: "HIER_EINTRAGEN",
};
```

Diese Werte sind NICHT geheim (Firebase-Web-Konfiguration ist öffentlich sichtbar
by design) - die eigentliche Absicherung passiert über die Regeln aus Teil 2.

---

## Fehlerbehebung

- **"Missing or insufficient permissions"**: die Regeln aus Teil 2 wurden nicht
  veröffentlicht, oder es ist noch der 30-Tage-Testmodus aktiv
- **Liste bleibt leer / "Verbinde..." hängt**: FIREBASE_CONFIG-Werte falsch
  eingetragen, oder Firestore wurde nicht in Teil 1 Schritt 3 angelegt
- **Nach 30 Tagen funktioniert nichts mehr**: Regeln in Teil 2 nochmal prüfen -
  der Standard-Testmodus läuft nach 30 Tagen automatisch ab
