# Design – 04 Ansicht «Gefährliche Aktionen»

Gemeinsame Architektur und Farbtoken: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec

### EventClassifier (F-5) — Backend `service`, Typen in `model`
Je Event → `DangerousAction` (`model`):
- `Shot` → immer; type `Tor` wenn `shot.outcome.name == 'Goal'`, sonst `Schuss`.
- `Pass` mit `pass.shot_assist` oder `pass.goal_assist` → `Schlüsselpass`.
- `Dribble` mit `outcome == 'Complete'` UND Start im Strafraum → `Dribbling im Strafraum`.
- `Carry` Start ausserhalb UND Ende im Strafraum → `Carry in den Strafraum`.
- Strafraum: `x >= 102 && 18 <= y <= 62`.
- Ausgeliefert über `GET /api/matches/{match_id}/players/{player_id}/dangerous-actions` (inkl. berechnetem xG + Referenz).

### Spielfeld (SVG) — Frontend React
120 × 80, Strafraum getönt, Markierungen aus Rechtecken/Kreisen. Symbole je Typ (Farbe nie allein):

| Typ | Symbol | Besonderheit |
| --- | --- | --- |
| Schuss | gefüllter Kreis | Grösse = berechnetes xG |
| Tor | gefüllter Kreis mit Ring | hervorgehoben |
| Schlüsselpass | gefülltes Quadrat | (Linie: Spec 05) |
| Dribbling im Strafraum | hohles Quadrat | – |
| Carry in den Strafraum | hohler Kreis | (Linie: Spec 05) |

Legende mit Zählern; Spielfeld ↔ Liste verknüpft; Tooltip «23′ · Tor, xG 0.25 (Referenz 0.19)».
Barrierefreiheit: Markierungen fokussierbar, Text mit Minute/Typ/Ergebnis.
