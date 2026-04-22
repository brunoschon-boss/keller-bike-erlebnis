# Keller Bike Erlebnis – Selbstgehostete Version

**Brand-Book-konform gebaut** (Keller Design Manual 2022).

## Was ist in diesem Paket

- `index.html` — Komplette Landingpage mit 5-Schritt-Formular (38 KB)
- `README.md` — Diese Anleitung

**Alle Bilder** werden direkt aus dem Supabase-Storage geladen (öffentliche URLs). Die Seite ist sofort einsatzbereit.

---

## 🎨 Brand-Book-Umsetzung

### Farben — nur die 6 definierten Farben

| Hex | Verwendung |
|---|---|
| `#DB0812` Rot | Signal-Farbe: CTA-Buttons, Akzent-Unterstriche, Focus-States (nie als Schriftfarbe) |
| `#000000` Schwarz | Haupttext, Headlines, Logo-Farbe |
| `#474746` Dunkelgrau | Sekundärtext, dunkle Flächen, Badges |
| `#909090` Mittelgrau | Tertiärtext, Eyebrows |
| `#CECECE` Hellgrau | Borders, Quote-Icons, Placeholders |
| `#FFFFFF` Weiß | Hintergrund, Schrift auf dunklen Flächen |

### Typografie — eine einzige Schrift-Familie

Aktuell: **Manrope** (Google Fonts, kostenlos) als nächste visuelle Annäherung an Mark Pro.

- Headlines: Manrope **Heavy (800)**, UPPERCASE
- Subline: Manrope **ExtraBold (800)**
- Fließtext: Manrope **Medium (500)** / Regular (400)

**Falls du die echte Mark Pro lizenziert hast:** Öffne `index.html`, such mit Strg+F nach `'Manrope'` (3 Stellen) und ersetze durch `'Mark Pro'`. Dann den Google-Fonts-Link (Zeile 9) durch deinen Mark-Pro-Einbindungscode austauschen (z.B. von Adobe Fonts).

### Eingehaltene Brand-Regeln ✅

- Keine rote Schrift — Rot nur als Fläche/Border/Unterstrich
- Keine Serif-Schriften
- Keine Outlines oder Schatten-Effekte auf Schrift
- Dunkelgrau/Schwarz auf Weiß (Standard) + Weiß auf Dunkelgrau (invertiert)
- Eine einzige Schrift-Familie für die gesamte Seite

---

## Schritt 1: Google Sheet einrichten (Lead-Empfang)

### 1.1 Neues Sheet anlegen

Geh auf [sheets.google.com](https://sheets.google.com) → neues leeres Sheet. Name z.B. "Keller Bike Leads".

In die **erste Zeile** (A1 bis H1) schreibst du genau diese Spaltenüberschriften:

| A | B | C | D | E | F | G | H |
|---|---|---|---|---|---|---|---|
| Timestamp | Interesse | Budget | Standort | Zeitraum | Name | Email | Telefon |

### 1.2 Apps Script hinterlegen

1. Im Sheet oben im Menü: **Erweiterungen → Apps Script**
2. Den Default-Code (`function myFunction()`) komplett löschen
3. Folgenden Code einfügen:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    sheet.appendRow([
      data.timestamp || new Date().toISOString(),
      data.interesse || '',
      data.budget || '',
      data.standort || '',
      data.zeitraum || '',
      data.name || '',
      data.email || '',
      data.telefon || ''
    ]);
    return ContentService.createTextOutput(JSON.stringify({status:'ok'}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({status:'error', message: err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Diskettensymbol zum Speichern klicken. Projektname beliebig, z.B. "Keller Lead Receiver".

### 1.3 Als Web-App bereitstellen

1. Oben rechts: **Bereitstellen → Neue Bereitstellung**
2. Beim Feld "Typ auswählen" das Zahnrad klicken → **Web-App**
3. Einstellungen:
   - **Beschreibung:** "Lead v1"
   - **Ausführen als:** *Ich (deine eigene Email)*
   - **Zugriff:** **Jeder** ⚠️ (wichtig!)
4. **Bereitstellen** klicken, Berechtigung zulassen
5. Am Ende bekommst du eine URL: `https://script.google.com/macros/s/AKfycbz.../exec`
6. URL kopieren.

### 1.4 URL in die HTML eintragen

In `index.html` suchst du (Strg+F / Cmd+F) nach:

```javascript
const GOOGLE_SCRIPT_URL = 'DEINE_GOOGLE_APPS_SCRIPT_URL_HIER';
```

Und ersetzt den Platzhalter durch deine URL. Die Hochkomma-Zeichen bleiben!

---

## Schritt 2: Hosting

### Option A: GitHub Pages (kennst du von Gut Boltenhof)

1. Neues Repo erstellen auf [github.com](https://github.com) (z.B. `keller-bike-erlebnis`)
2. `index.html` hochladen
3. Repo-Einstellungen → Pages → Branch `main` + `/ (root)` → Save
4. Nach 1–2 Minuten live unter `https://DEIN-USERNAME.github.io/keller-bike-erlebnis`

### Option B: Eigene Domain

Wenn du `www.keller-bike-erlebnis.de` wieder nutzen willst:
1. Erst hosten (z.B. GitHub Pages)
2. Dann DNS beim Domain-Anbieter umziehen
3. Besprechen wir separat, sobald Hosting steht

---

## Testen nach Upload

1. Seite öffnen → alles sollte in Schwarz/Grau/Weiß mit roten Akzenten erscheinen
2. Auf "Termin vereinbaren" klicken
3. Alle 5 Schritte mit Test-Daten durchgehen
4. **Prüfe in Google Sheets:** Lead erscheint als neue Zeile ✅

Fehlt der Lead?
- Browser-Konsole öffnen (F12) → bei Step 5 nochmal submitten → Fehler lesen
- Häufigster Fehler: bei der Web-App-Bereitstellung "Zugriff: Jeder" nicht gesetzt

---

## Bilder absichern (optional, für später)

Aktuell liegen die 8 Bilder auf dem Supabase-Storage. Falls das irgendwann abgeschaltet wird:

1. Logo → speichern als `logo.png`
2. Hero (Radfahrer am See) → `hero.png`
3. Bikefitting → `bikefitting.png`
4. Stau → `stau.png`
5. Seebrise → `seebrise.png`
6. Körpergefühl → `koerpergefuehl.png`
7. Frühjahrssale → `sale.png`
8. Showroom → `showroom.png`

Alle in einen Ordner `images/` neben der `index.html` legen. Dann in der HTML mit Strg+F nach `supabase.co` suchen und URLs durch `images/logo.png` usw. ersetzen. Sag Bescheid — ich mach das dann für dich in einem Rutsch.

---

## Formular-Felder (damit du weißt, was gesendet wird)

- **Schritt 1:** Interesse (E-Bike / Rennrad / Mountainbike / Kinderrad / Unsicher)
- **Schritt 2:** Budget (unter 1.000 € / 1.000–2.500 € / 2.500–5.000 € / 5.000 € + / Flexibel)
- **Schritt 3:** Standort (Friedrichshafen / Wangen / Egal)
- **Schritt 4:** Zeitraum (Wochentag vormittags / nachmittags / Samstag / Flexibel)
- **Schritt 5:** Name, Email, Telefon
- **Automatisch:** Timestamp

---

Bei Fragen einfach melden! 🚴‍♂️
