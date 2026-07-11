# VIDA Salon — Kundenkartei & Terminkalender (cSALON01)

App für die **VIDA Hair Lounge**: Kundenkartei (mit Farbrezepturen, Hinweisen zu
Unverträglichkeiten und Besuchshistorie) und Terminkalender — als **PWA** für
iPad und iPhone. Alle Daten bleiben **verschlüsselt auf dem Gerät**.

## Dateien

| Datei | Zweck |
|---|---|
| `VIDA-Salon.html` | Die komplette App (eine Datei, keine externen Verbindungen) |
| `manifest.webmanifest` | PWA-Manifest (App-Name, Icons, Vollbildmodus) |
| `sw.js` | Service Worker → App läuft offline |
| `icon-180/192/512.png` | App-Icons (erzeugt von `generate_icons.py`) |

## Installation auf iPad / iPhone

Die App muss einmalig über **https** (oder im lokalen Netz über einen kleinen
Webserver) geöffnet werden — z. B. die Dateien auf einen Webspace legen
(Unterordner von `paulec.eu` genügt) oder per iCloud/AirDrop übertragen und
lokal hosten.

1. **Safari** öffnen und die Adresse der `VIDA-Salon.html` aufrufen
2. **Teilen-Symbol** (Quadrat mit Pfeil) → **„Zum Home-Bildschirm"**
3. Die App erscheint als **VIDA**-Icon und läuft ab dann **offline** und im Vollbild
4. Beim ersten Start eine **PIN** (mind. 4 Ziffern) festlegen

> ⚠️ **PIN vergessen = Daten unwiederbringlich verloren.** Es gibt absichtlich
> keine Hintertür. Deshalb: regelmäßig Backups exportieren!

## Abgleich iPad ↔ iPhone (manuell)

Die Daten liegen nur lokal — es gibt keine automatische Synchronisation
(bewusste Datenschutz-Entscheidung).

1. Auf dem Hauptgerät (z. B. iPad): **Mehr → Daten übertragen**
2. Datei per **AirDrop** (oder iCloud Drive) ans andere Gerät senden
3. Dort **„In Dateien sichern"** antippen — die Datei hat die Endung `.vida`,
   iOS öffnet sie deshalb **nicht** in einer Vorschau
4. VIDA öffnen: **Mehr → Daten empfangen** → Datei wählen →
   **Zusammenführen** (neuere Einträge gewinnen) oder **Ersetzen**
   (gleiche PIN auf beiden Geräten = keine PIN-Eingabe nötig)

Ältere Backups mit Endung `.json` lassen sich weiterhin importieren.

Empfehlung: das **iPad als führendes Gerät** im Salon nutzen und das iPhone
z. B. wöchentlich per Backup aktualisieren.

## Datenschutz (DSGVO)

- Daten verlassen das Gerät **nie** — kein Server, kein CDN, keine Analytics
- Speicherung **AES-256-GCM-verschlüsselt** (Schlüssel aus PIN per PBKDF2, 310 000 Iterationen)
- **Auto-Sperre** nach Inaktivität (einstellbar unter „Mehr")
- **Löschfunktion** je Kundin (Recht auf Löschung, Art. 17 DSGVO) — entfernt auch alle Termine
- **CSV-Export** der Kundenliste für Auskunftsersuchen (Art. 15 DSGVO) — unverschlüsselt, mit Warnhinweis
- Zusätzlich empfohlen: Gerätesperre (Code/Face ID) aktivieren; Backups sicher aufbewahren
- Termin-Erinnerungen/Push wurden bewusst weggelassen (in iOS-PWAs unzuverlässig)

## Hinweis zur Datenhaltung

iOS kann Website-Daten löschen, wenn eine **im Browser** geöffnete Seite lange
nicht genutzt wird. Als **installierte Home-Bildschirm-App** sind die Daten
davon in der Praxis nicht betroffen; die App fordert zusätzlich dauerhafte
Speicherung an (`navigator.storage.persist`). Trotzdem gilt: **regelmäßig
Backup exportieren.**

## Updates: neue Version, alte Daten behalten

Die Daten (Kundinnen, Termine, Karteikarten, Dienstleistungen, Einstellungen,
PIN) liegen **nicht in der HTML-Datei**, sondern in der IndexedDB des Browsers —
gebunden an die **Domain**, unter der die App läuft.

- **Normalfall:** Die neue `VIDA-Salon.html` einfach unter **derselben Adresse**
  auf den Webspace legen (Datei überschreiben) und in `sw.js` die Zeile
  `const CACHE = 'vida-salon-vX'` **um eins hochzählen** (ebenso `APP_VERSION`
  in `VIDA-Salon.html` — die Nummer steht unten im „Mehr"-Tab, so lässt sich
  am Gerät prüfen, ob das Update angekommen ist). Die installierte App
  lädt beim nächsten Öffnen (ggf. einmal schließen und neu öffnen) die neue
  Version — **alle Daten bleiben automatisch erhalten**, kein Export nötig.
- **Homescreen-Icon niemals löschen:** iOS entfernt beim Löschen einer
  Homescreen-App auch **alle lokalen Daten** (komplette Kundenkartei!).
  Für ein Update ist das Löschen nie nötig.
- **Sicherheitsnetz:** Vor jedem Update trotzdem einmal **Daten übertragen**
  (= verschlüsseltes Backup) ausführen und die `.vida`-Datei aufheben.
- **Nur bei Domain-Wechsel** (z. B. Umzug auf eine andere Adresse) sieht die
  neue Installation die alten Daten nicht — dann das Backup dort über
  **Daten empfangen** importieren. Neue Versionen müssen das Backup-Format
  `vida-salon-backup` (Version 1) weiterhin lesen können.

## Entwicklung

- Icons neu erzeugen: `python generate_icons.py` (benötigt Pillow)
- Lokal testen: beliebiger statischer Server im Ordner `cSALON01`, dann
  `http://localhost:PORT/VIDA-Salon.html`
