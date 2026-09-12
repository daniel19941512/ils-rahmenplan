# Rahmenplan-Werkstatt mit Google Drive – Einrichtung

Zwei Dateien liegen bei: `index.html` (die ganze App) und diese Anleitung.
Du musst zwei Werte in `index.html` eintragen, dann alles zu GitHub hochladen.

---

## Teil 1: Google Cloud – OAuth Client ID erstellen (ca. 10 Minuten)

1. Gehe zu **https://console.cloud.google.com/**
2. Oben ein neues Projekt anlegen (z.B. "ILS-Rahmenplan") oder ein bestehendes nutzen.
3. Menü links → **"APIs & Dienste" → "Aktivierte APIs und Dienste"** → **"+ APIS UND DIENSTE AKTIVIEREN"**
   → nach **"Google Drive API"** suchen → aktivieren.
4. Menü links → **"APIs & Dienste" → "OAuth-Zustimmungsbildschirm"**
   - Nutzertyp: **"Extern"** (falls kein Google Workspace) oder **"Intern"** (falls Workspace-Domain)
   - App-Name: z.B. "ILS Rahmenplan-Werkstatt", eigene E-Mail als Support-Kontakt eintragen
   - Bei "Testnutzer": **alle E-Mail-Adressen eintragen, die die App später benutzen sollen** (bis zu 100 möglich)
   - Speichern. Die App bleibt im Status "Testing" – das reicht für internen Gebrauch,
     Google muss die App NICHT extra prüfen. Nutzer sehen beim ersten Anmelden eine
     Warnung "Diese App wurde nicht verifiziert" – auf "Erweitert" → "Trotzdem fortfahren" klicken, das ist normal.
5. Menü links → **"APIs & Dienste" → "Anmeldedaten"** → **"+ ANMELDEDATEN ERSTELLEN"** → **"OAuth-Client-ID"**
   - Anwendungstyp: **"Webanwendung"**
   - Name: beliebig
   - Bei **"Autorisierte JavaScript-Quellen"**: die spätere GitHub-Pages-Adresse eintragen,
     z.B. `https://DEIN-GITHUB-NAME.github.io` (ohne Pfad dahinter, ohne Schrägstrich am Ende).
     Falls du die Adresse noch nicht kennst: Teil 3 zuerst machen, dann hier ergänzen
     (kann jederzeit nachträglich hinzugefügt werden).
   - Erstellen → die **Client-ID** wird angezeigt (endet auf `.apps.googleusercontent.com`) → kopieren

---

## Teil 2: Ordner-ID herausfinden

Öffne euren gemeinsamen Google-Drive-Ordner im Browser. Die Adresse sieht so aus:

```
https://drive.google.com/drive/folders/1AbCdEfGhIjKlMnOpQrStUvWxYz
                                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                        das ist die Ordner-ID
```

Den Teil nach `/folders/` kopieren.

---

## Teil 3: Werte eintragen

In `index.html` ganz oben im Skript-Teil diese zwei Zeilen suchen und anpassen:

```js
const GOOGLE_CLIENT_ID = "HIER_DEINE_CLIENT_ID_EINFUEGEN.apps.googleusercontent.com";
const DRIVE_FOLDER_ID = "HIER_DEINE_ORDNER_ID_EINFUEGEN";
```

Ersetzen durch eure Werte aus Teil 1 und Teil 2.

---

## Teil 4: Auf GitHub veröffentlichen

1. Auf **https://github.com** ein neues Repository erstellen (z.B. `ils-rahmenplan`), öffentlich oder privat.
2. `index.html` in das Repository hochladen (im Browser: "Add file" → "Upload files").
3. Im Repository → **"Settings" → "Pages"**
   - Unter "Source": **"Deploy from a branch"**, Branch **"main"**, Ordner **"/ (root)"** → Speichern
4. Nach 1-2 Minuten ist die App erreichbar unter:
   ```
   https://DEIN-GITHUB-NAME.github.io/ils-rahmenplan/
   ```
5. Diese Adresse zurück in Google Cloud eintragen (Teil 1, Schritt 5, "Autorisierte JavaScript-Quellen"),
   falls noch nicht geschehen.

---

## Berechtigungen der Kollegen

Jede Person, die die App nutzen soll:
- Muss bei den Google-Testnutzern eingetragen sein (Teil 1, Schritt 4)
- Muss Zugriff auf den Drive-Ordner haben (normal in Drive freigeben, wie gewohnt)

## Fehlerbehebung

- **"Diese App wurde nicht verifiziert"** beim Anmelden: normal im Testmodus, auf "Erweitert" → "Trotzdem fortfahren"
- **"Zugriff verweigert" / Ordner leer**: die anmeldende Person hat keinen Zugriff auf den Drive-Ordner – in Drive freigeben
- **Anmelde-Button tut nichts**: Client-ID falsch eingetragen, oder die GitHub-Pages-Adresse fehlt bei "Autorisierte JavaScript-Quellen"
