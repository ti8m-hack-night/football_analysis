# Design – 02 Auswahl der Analyse

Gemeinsame Architektur: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
- **Frontend (React):** Auswahl-Sheet (Handy von unten, Desktop zentriert); Default FIFA World Cup 2022 (43/106), letztes Spiel. Daten aus `GET /api/competitions` und `GET /api/matches`.
- **Backend (`service`/`api`):** Spielerliste aus `events[].player` eindeutig ableiten (`GET /api/matches/{match_id}/players`); Default = Spieler mit den meisten gefährlichen Aktionen.
- Auswahländerung triggert reaktives Neuladen/Neurendern im Frontend (A-1), kein Reload; neuer REST-Aufruf statt Codeänderung.
