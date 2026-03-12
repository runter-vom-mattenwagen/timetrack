# Prompt: Time Tracking PWA

Erstelle eine vollständige, installierbare Progressive Web App (PWA) zur Zeiterfassung als **einzelne HTML-Datei** mit eingebettetem CSS und JavaScript. Kein Framework, kein Build-Step — pure Vanilla JS.

## Kernfunktionalität

### Projektverwaltung
- Flache Projektliste (kein Nesting/Kategorien)
- Projekte hinzufügen (Name + optionale Farbe zur visuellen Unterscheidung)
- Projekte entfernen (mit Bestätigungsdialog, da zugehörige Zeiteinträge gelöscht werden)
- Projekte umbenennen

### Favoriten-System
- Jedes Projekt kann als Favorit markiert/entmarkiert werden (z.B. Stern-Icon in der Projektliste)
- **Favoriten-Bereich:** Oben auf der Hauptseite, prominent sichtbar, als große klickbare Buttons/Cards dargestellt
- Jeder Favoriten-Button zeigt: Projektname, Projektfarbe, und den Start/Stop-Toggle für den Timer
- So hat man die täglichen Kernprojekte mit einem Tap erreichbar, ohne durch die vollständige Liste scrollen zu müssen
- Darunter: Vollständige Projektliste (inklusive Favoriten-Toggle) für Verwaltung und selten genutzte Projekte
- Favoriten-Status wird in den Projektdaten persistiert

### Timer
- Jedes Projekt hat einen eigenen Start/Stop-Toggle-Button
- Starten eines Timers beendet einen anderen laufenden
- Laufende Timer zeigen die aktuelle Dauer in Echtzeit an (live tickend, sekündlich aktualisiert)
- Visuell klar erkennbar, welche Timer gerade aktiv sind (z.B. pulsierender Indikator, farbliche Hervorhebung)
- Timer-State überlebt Page-Reload (Startzeit wird persistiert, Dauer wird beim Laden neu berechnet)

### Manuelle Zeiteinträge
- Zusätzlich zum Timer: Möglichkeit, Zeiteinträge manuell zu erfassen
- Eingabefelder: Projekt (Dropdown), Datum, Startzeit, Endzeit
- Validierung: Endzeit muss nach Startzeit liegen, kein Datum in der Zukunft

### Zeitübersicht
- **Tagesansicht (Default):** Alle Einträge des gewählten Tages, gruppiert nach Projekt, mit Einzelzeiten und Tagessumme pro Projekt sowie Gesamtsumme des Tages
- **Wochenansicht:** Tabellarische Darstellung — Projekte als Zeilen, Wochentage als Spalten, Summen pro Zeile und Spalte. Kalenderwochen-Navigation (vor/zurück)
- **Monatsansicht:** Projekte als Zeilen, Tage als Spalten (oder kompakter: Wochen als Spalten mit Wochensummen). Monats-Navigation (vor/zurück)
- Zeitformat: `HH:MM:SS` für laufende Timer, `HH:MM` für abgeschlossene Zusammenfassungen
- Tagesgrenze ist Kalendertag (00:00–23:59). Timer, die über Mitternacht laufen, werden auf den jeweiligen Tag aufgeteilt.

## Datenhaltung

### localStorage
- Alle Daten (Projekte, Zeiteinträge, laufende Timer, Einstellungen) werden im `localStorage` gespeichert
- Datenstruktur als JSON mit klarer Trennung:
  ```
  {
    "projects": [{ "id": "uuid", "name": "...", "color": "#...", "favorite": true/false }],
    "entries": [{ "id": "uuid", "projectId": "...", "start": "ISO8601", "end": "ISO8601" }],
    "activeTimers": [{ "projectId": "...", "startedAt": "ISO8601" }],
    "settings": { "theme": "auto" }
  }
  ```
- UUIDs via `crypto.randomUUID()`

### Import/Export
- **Export:** Gesamte Daten als JSON-Datei herunterladen (Dateiname: `timetrack-export-YYYY-MM-DD.json`)
- **Import:** JSON-Datei hochladen. Vor dem Import: Auswahl ob bestehende Daten **ersetzt** oder **gemergt** werden (beim Merge: Duplikaterkennung via ID)
- Validierung der Import-Datei (Strukturprüfung, Fehlermeldung bei ungültigem Format)

## UI/UX Design

### Layout
- Mobile-first, responsive (funktioniert auf Smartphone und Desktop)
- Hauptansicht von oben nach unten: **Favoriten-Buttons** (große, prominente Timer-Cards) → **Tagesübersicht** → Link/Button zur vollständigen Projektliste
- Navigation zwischen Tages-/Wochen-/Monatsansicht über Tabs oder Segmented Control
- Einstellungen und Import/Export über ein Menü oder Settings-Seite

### Dark Mode / Light Mode
- Automatische Erkennung via `prefers-color-scheme`
- Optionaler manueller Toggle (Auto / Light / Dark) in den Einstellungen
- Saubere CSS-Variablen für Theming, kein hartcodiertes Styling

### Allgemein
- Clean, minimalistisches Design — kein visuelles Rauschen
- Gut lesbare Typografie, ausreichend Kontrast in beiden Themes
- Touch-freundliche Buttons (min. 44px Tappable Area)
- Smooth Transitions bei Theme-Wechsel und View-Wechsel
- Kein generisches AI-Look. Nutze eine markante, eigenständige Designsprache: durchdachte Farbpalette, charaktervolle (aber gut lesbare) Typografie, bewusster Einsatz von Whitespace.

## PWA-Anforderungen

### Manifest (manifest.json)
- `name`: "TimeTrack"
- `short_name`: "TimeTrack"
- `display`: "standalone"
- `start_url`: "./index.html"
- `theme_color` und `background_color` passend zum Design
- Icons in den gängigen Größen (als inline SVG im Manifest oder als Data-URI generiert)

### Service Worker
- Caching der HTML-Datei für vollständige Offline-Fähigkeit
- Cache-First-Strategie (App läuft komplett offline)
- Versioned Cache (bei Updates wird alter Cache gelöscht)

### Da alles in einer HTML-Datei sein soll:
- Das `manifest.json` und der Service Worker (`sw.js`) werden beim ersten Laden dynamisch als Blob-URLs registriert, ODER
- Manifest wird als Data-URI im `<link rel="manifest">` eingebettet, Service Worker als separates inline-generiertes Script

## Technische Constraints
- **Eine einzige HTML-Datei** — alles inline (CSS, JS, SVG-Icons)
- Vanilla JavaScript (ES2020+), kein Framework, keine externen Dependencies
- Keine Build-Tools, kein Bundler
- Semantisches HTML5
- Kein `eval()`, kein `innerHTML` für User-Input (XSS-sicher)
- Performant: requestAnimationFrame oder setInterval(1000) für Timer-Updates, nicht schneller

## Lieferergebnis
- Eine einzige `index.html`-Datei, die als PWA installierbar ist und vollständig offline funktioniert
- Sauberer, kommentierter Code
- Sofort lauffähig — File im Browser öffnen und fertig
