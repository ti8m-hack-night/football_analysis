# Design – 01 Daten live laden

Gemeinsame Architektur: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
- `StatsBombClient` (`infrastructure`): HTTP-`fetch` gegen die Basis-URL, keine eingebetteten Datensätze.
- `getCompetitions()` beim ersten Zugriff über `GET /api/competitions` (Backend).
- `getEvents(matchId)` erst bei Spielwahl, über die Spieler-/Aktions-Endpunkte.
- Verifikation: `grep` nach eingebetteten Match-/Event-Daten im Backend- und Frontend-Quellcode muss leer sein (F-1).
