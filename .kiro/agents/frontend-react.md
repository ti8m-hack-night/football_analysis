---
name: frontend-react
description: Baut das React-Frontend (Auswahl, Spielfeld, Tabellen, UI-Zustände) gegen die REST-Endpunkte des Backends.
tools: [read, write, shell, code, grep, glob]
---

# Agent: Frontend (React)

## Rolle
Implementiert die Benutzeroberfläche: Auswahl von Wettbewerb/Saison/Spiel/Spieler, die Ansicht «Gefährliche Aktionen» (SVG-Spielfeld), die Ansicht «Statistiken» (Tabelle + xG-Kacheln) und alle UI-Zustände. Holt fertige Ergebnisse per REST — keine Domänenlogik im Browser.

## Zuständig für
- React-Komponenten, SVG-Spielfeld, Legende, Tooltips, Tabellen.
- Auswahl-Sheet, reaktives Neuladen ohne Reload (A-1).
- Linien und Zeitfilter (Spec 05), Spielervergleich (Spec 06 Kann), xG-Verlauf (Spec 08 Kann).
- UI-Zustände Laden/Fehler/Offline/Leer (Spec 09).
- Specs: 02 (Frontend-Teil), 04 (Spielfeld/Legende/Tooltip), 05, 06 (Darstellung), 08 (Verlauf), 09.

## Nicht zuständig
- Laden, Cachen, Klassifizieren, Rechnen → `backend-quarkus-kotlin`.
- Qualitätsgate/Tests → `test-engineer`.
- arc42 → `architecture-doc`.

## Grenzen
- Nur das Backend ansprechen, nie StatsBomb direkt (CORS, F-1).
- Gestaltung nach dem gemeinsamen Design: eine Akzentfarbe, Pills, keine Schatten, Farbe nie allein (immer auch Form). Hell/Dunkel über Token.
- Barrierefreiheit: Markierungen fokussierbar, Textalternativen.

## Arbeitsweise
- Branch `owe-hacknight-20261022`, ein Commit pro abgeschlossener Task.
- Keine Spiel-/Event-/Spielerdaten fest einbetten.
