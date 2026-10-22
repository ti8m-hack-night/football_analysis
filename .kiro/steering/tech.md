# Technische Grenzen

## In Scope
- Quarkus-Backend (Kotlin) lädt StatsBomb-Daten, cached sie, klassifiziert, rechnet Kennzahlen und xG.
- React-Frontend holt fertige Ergebnisse per REST und zeigt sie an (keine Domänenlogik im Browser).
- Daten **live** aus dem Internet: StatsBomb Open Data (`https://raw.githubusercontent.com/statsbomb/open-data/master/data/`), kein API-Key.
- Server-seitiger Cache im Backend (In-Memory / lokale Datei-Ablage, keine Datenbank).
- xG wird im **Backend** berechnet (Referenzmodell als reine Kotlin-Funktion im `model`-Modul, siehe Spec 07).

## Out of Scope (nicht gefordert)
- Login / Authentifizierung
- Datenbank (Server-seitig persistent)
- Deployment, Hosting, CI/CD
- Tests über die Abnahmekriterien A-1 bis A-7 hinaus (Ausnahme: ArchUnit-Architekturtest)

## Auflagen
- Bei Veröffentlichung StatsBomb als Quelle nennen (Nutzungsbedingungen des Repos).
- Keine Spiel-, Event- oder Spielerdaten fest im Code einbetten (F-1).

## Technologie
- Wir verwenden Quarkus mit Kotlin als Framework.
- Im UI setzen wir auf React. Das Frontend spricht nur das eigene Backend an, nie StatsBomb direkt.
- Wir setzen stark auf Modularisierung. Stelle mittels Archunit sicher, dass diese Abhängigkeiten gemäss Onion-Architektur eingehalten werden.
  - folgende Module existieren: model, api, infrastructure, service
  - Abhängigkeiten nach innen: `api` → `service` → `model`; `infrastructure` → `model`, `service`; `model` → nichts.

## API-Standards
- REST-APIs folgen den [Zalando RESTful API Guidelines](https://github.com/zalando/restful-api-guidelines).
- Insbesondere: Kebab-Case in Pfaden, `snake_case` in JSON-Feldern, sinnvolle HTTP-Statuscodes, Problem-JSON (RFC 9457) für Fehler, Pluralnamen für Collections, keine Verben in Pfaden.
- Gilt für alle `/api/...`-Endpunkte (siehe `.kiro/specs/_shared/design.md`).
