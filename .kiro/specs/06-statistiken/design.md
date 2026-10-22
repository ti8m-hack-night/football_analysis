# Design – 06 Ansicht «Statistiken»

Gemeinsame Architektur: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec

### StatsCalculator (F-10) — Backend `service`, `PlayerStats` in `model`
Je Spieler: Schüsse, Schüsse aufs Tor (`Goal`,`Saved`,`Saved to Post`), Tore (== `Goal`-Events, A-2), Pässe, angekommene Pässe (ohne `pass.outcome`), Passquote %, Schlüsselpässe, erfolgreiche Dribblings (alle, nicht nur Strafraum), xG berechnet (Spec 07), xG Referenz (`shot.statsbomb_xg`). Ausgeliefert über `GET /api/matches/{match_id}/players/{player_id}/stats`.

### Teamvergleich (F-11) — Backend `service`
Durchschnitt pro Spieler über alle Teamspieler mit Events. Team-Passquote = Summe aller Pässe / Summe angekommener Pässe.

### Spielervergleich (F-12, Kann) — Frontend
Zweite Spalte mit zweitem Spieler, nebeneinander (zweiter REST-Aufruf).

### Darstellung — Frontend React
Drei Kacheln xG berechnet / Referenz / Abweichung. Tabelle mit tabellarischen Ziffern.
