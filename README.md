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

1. Auf dem Hauptgerät (z. B. iPad): **Mehr → Verschlüsseltes Backup exportieren**
2. Datei per **AirDrop** (oder iCloud Drive) ans andere Gerät senden
3. Dort: **Mehr → Backup importieren** → PIN des Backups eingeben →
   **Zusammenführen** (neuere Einträge gewinnen) oder **Ersetzen**

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

## Entwicklung

- Icons neu erzeugen: `python generate_icons.py` (benötigt Pillow)
- Lokal testen: beliebiger statischer Server im Ordner `cSALON01`, dann
  `http://localhost:PORT/VIDA-Salon.html`
