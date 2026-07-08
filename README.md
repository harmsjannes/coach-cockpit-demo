# Coach-Cockpit — Projekt-Kontext

Demo-Prototyp für Markus Häusler (Silvester Neidhardt / Körper-Neustart Programm). Dies ist die technische Bewerbungsaufgabe für die AI-Wizard-Rolle.

**Live-Demo:** https://harmsjannes.github.io/coach-cockpit-demo/

---

## Status

Aktuell: **statischer HTML/CSS-Prototyp**, rein visuell. Kundenliste links ist nicht interaktiv — dient nur zum Zeigen des Designs und der Idee gegenüber Markus. Kein Backend, keine echten Daten, keine Funktionalität.

**Nächster Schritt:** Interaktivität einbauen (Klick auf Kunde in der Liste wechselt die rechte Detailansicht), damit es sich wie echte Software anfühlt statt wie ein Standbild.

---

## Was das Projekt ist

Ein **Coach-Cockpit**: internes Werkzeug für Coaches, um alle Kundendaten (Check-ins, KKS-Score-Verlauf, Signale wie Schmerz/Hilfebedarf) an einer Stelle zu sehen. Ersetzt das manuelle Zusammensuchen aus mehreren Quellen.

**Wichtig — zwei getrennte Systeme:**
- **Mitgliederbereich** (sn-coaching-neu.netlify.app, bestehend, Airtable-Backend): Kunden loggen sich ein, sehen eigene Daten. Bleibt unverändert.
- **Coach-Cockpit** (dieses Projekt, neu): Coaches loggen sich ein, sehen alle ihre Kunden. Komplett neuer Stack.

Vollständige Konzeption (Anforderungen, Datenmodell, Arbeitspakete, Stundenschätzung) liegt separat als `Coach-Cockpit_Konzeption_Jannes.md` vor — bei Bedarf nachreichen/verlinken.

---

## Tech-Entscheidungen (final für die echte Umsetzung)

- **Supabase (PostgreSQL)** statt Airtable — volle Kontrolle, Row-Level-Security (Coach sieht nur eigene Kunden), komplexe Trend-Queries
- **React** statt Low-Code-GUI — für die echte App (dieser Prototyp hier ist bewusst reines HTML/CSS, weil er nur zum schnellen Zeigen gedacht war, kein Build-Prozess nötig)
- **Node.js/Express API** statt n8n — eigene Erfahrung: n8n limitiert bei komplexer Logik, Custom Code ist schneller und sauberer (siehe Erfahrung mit Clara-Projekt)
- **Eigenes React-Formular** statt JotForm für den Check-in-Eingang — volle Kontrolle, sauber integriert
- **Prinzip:** Niemand (Silvester, Markus, Coaches) fasst Supabase direkt an. Alles läuft über Frontend → API → Datenbank. Keine manuellen SQL-Befehle.

## Datenmodell (Kurzfassung)

```
coaches → customers (1:N, RLS: coach_id = auth.uid())
customers → checkins (1:N)
customers → kks_screenings (1:N)
```

Details (Feldstruktur Check-in, KKS-Score-Logik 0–165 Punkte, Kategorien HWS/BWS/LWS) siehe Konzeption.

## Arbeitspakete (aus Konzeption, ~25h MVP)

A. Supabase Setup + RLS · B. Datenmigration Airtable → Supabase · C. React Check-in-Formular · D. Node.js API · E. Dashboard (Kundenübersicht + Akte) · F. KKS-Verlauf-Grafik · G. Signale/Rot-Flags · H. Testing & Polish

---

## Design-Entscheidungen (dieser Prototyp)

Bewusst **kein** generischer KI-Look (kein Cream+Terracotta, kein Dark-Mode-Neon-Akzent):

- Farbe: Ink-Blau `#1C2B39` (angelehnt an Silvesters Logo, gedeckter), warmes Off-White `#F3F4F1`, gedecktes Ziegelrot für Signale, gedecktes Salbeigrün für Fortschritt
- Typografie: Space Grotesk (Display) + Inter (Body) + IBM Plex Mono (Zahlen/Scores — wirken wie Messwerte)
- Signatur-Element: KKS-Score als Rundinstrument/Gauge (spielt mit dem Namen "Cockpit")

---

## Womit weiterarbeiten

Dieses Repo lokal öffnen (`coach-cockpit-demo/`), dann mit Claude Code oder Codex direkt an `index.html` weiterbauen. Sinnvolle nächste Schritte, in Reihenfolge:

1. Klick-Interaktivität: Kundenzeile klicken → Detailansicht rechts wechselt (reines JavaScript, kein Framework nötig für den Prototyp)
2. Mobile-Check: Layout auf schmalen Viewports testen/fixen
3. Optional: 2-3 weitere Kunden-Datensätze für mehr Realismus in der Demo
4. Erst wenn Markus grünes Licht gibt: echter Umbau auf React + Supabase gemäß Konzeption (nicht vorher, um keine Zeit in die falsche Richtung zu stecken)
