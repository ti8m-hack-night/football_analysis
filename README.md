# football_analysis

Analysiere einen Fussballspieler in einem Spiel: gefährliche Aktionen auf dem Spielfeld, Statistiken und xG-Berechnung.

> **UX-Prototyp:** _Link folgt (wird vor der Hack Night bereitgestellt)_
> **Zeitrahmen:** 1 Stunde. Priorisiert: zuerst alle **Muss**-Anforderungen, dann **Soll**, dann **Kann**.
> **Technologie, Git:** frei wählbar bzw. optional. Die Anwendung läuft **lokal**, die Daten werden aus dem **Internet** geladen.

## 1. Datenquelle (Pflicht)

[StatsBomb Open Data](https://github.com/statsbomb/open-data), kein API-Key nötig. Basis-URL:

```
https://raw.githubusercontent.com/statsbomb/open-data/master/data/
```

| Zweck | Pfad |
|---|---|
| Wettbewerbe und Saisons | `competitions.json` |
| Spiele einer Saison | `matches/{competition_id}/{season_id}.json` |
| Events eines Spiels | `events/{match_id}.json` |
| Aufstellungen | `lineups/{match_id}.json` |

Empfohlener Einstieg: FIFA World Cup 2022 (`competition_id` 43, `season_id` 106). Die Spiele ermitteln Sie über die Matches-Datei.

**Koordinaten:** Spielfeld 120 × 80, Angriffsrichtung immer nach rechts (x = 120 ist das gegnerische Tor). Das Tor liegt bei x = 120, y = 36 bis 44. Der Strafraum liegt bei x ≥ 102 und y zwischen 18 und 62.

**Relevante Events:** `type.name` ist `Shot`, `Pass`, `Dribble` oder `Carry`. Schüsse haben `shot.outcome.name` (`Goal`, `Saved`, `Off T`, …), `shot.statsbomb_xg` (Referenz-xG) und `shot.type.name` (`Open Play`, `Penalty`, …). Pässe haben `pass.shot_assist` oder `pass.goal_assist`. Ein Pass ohne `pass.outcome` gilt als angekommen.

**Auflage:** Bei Veröffentlichung ist StatsBomb als Quelle zu nennen (siehe Nutzungsbedingungen im Repository).

## 2. Anforderungen

### Daten laden
- **F-1 (Muss):** Die App lädt Wettbewerbe, Saisons, Spiele und Events live von der Quelle. Es werden keine Daten fest einkompiliert.
- **F-2 (Muss):** Der Nutzer wählt Wettbewerb, Saison, Spiel und Spieler (Auswahl aus den Spielern, die im Spiel Events haben).
- **F-3 (Muss):** Geladene Antworten werden lokal zwischengespeichert, damit ein erneuter Aufruf ohne Netz funktioniert. Ladefehler werden verständlich angezeigt, die App stürzt nicht ab.

### Ansicht 1: Gefährliche Aktionen
- **F-4 (Muss):** Ein Spielfeld zeigt alle gefährlichen Aktionen des gewählten Spielers an den Positionen, an denen sie stattfanden.
- **F-5 (Muss):** Als gefährlich gelten:
  - jeder Schuss
  - jeder Pass mit `pass.shot_assist` oder `pass.goal_assist`
  - jedes erfolgreiche Dribbling (`Dribble` mit `outcome` `Complete`), das im Strafraum stattfindet
  - jeder `Carry`, der ausserhalb des Strafraums beginnt und im Strafraum endet
- **F-6 (Muss):** Die Aktionstypen sind farblich oder per Symbol unterscheidbar, und es gibt eine Legende. Tore sind hervorgehoben.
- **F-7 (Muss):** Ein Tooltip oder Detailbereich zeigt Minute, Aktionstyp und Ergebnis, bei Schüssen auch xG.
- **F-8 (Soll):** Pässe und Carries werden als Linie vom Start- zum Endpunkt gezeichnet (`location` und `end_location`).
- **F-9 (Kann):** Ein Zeitfilter zeigt nur Aktionen innerhalb einer Minutenspanne.

### Ansicht 2: Statistiken
- **F-10 (Muss):** Eine Tabelle für den gewählten Spieler zeigt: Schüsse, Schüsse aufs Tor (`Goal`, `Saved`, `Saved to Post`), Tore, Pässe, angekommene Pässe, Passquote in %, Schlüsselpässe (`shot_assist` oder `goal_assist`), erfolgreiche Dribblings, xG (berechnet) und xG (Referenz).
- **F-11 (Soll):** Der Spieler wird mit dem Teamdurchschnitt des Spiels verglichen.
- **F-12 (Kann):** Ein Vergleich zweier Spieler nebeneinander.

### xG-Berechnung
- **F-13 (Muss):** Für jeden Schuss wird xG selbst berechnet und nicht aus `shot.statsbomb_xg` übernommen. Dieser Wert dient nur als Referenz.
- **F-14 (Muss):** Das Referenzmodell der Aufgabe:
  - Elfmeter (`shot.type.name` = `Penalty`): xG = 0.78
  - alle anderen Schüsse (Schussposition x, y; x' = 120 − x):
    - Distanz `d = sqrt(x'² + (y − 40)²)`
    - Winkel `a = atan2(8·x', x'² + (y − 40)² − 16)` in Radiant
    - `xG = 1 / (1 + e^−(−1.0 − 0.12·d + 2.0·a))`
- **F-15 (Muss):** Die Summe der xG-Werte wird pro Spieler und im Vergleich zur Referenzsumme angezeigt (absolute und prozentuale Abweichung).
- **F-16 (Soll):** Das Modell lässt sich verbessern oder austauschen (z. B. Körperteil, Schussart). Die Abweichung zur Referenz ist dann zu begründen.
- **F-17 (Kann):** Ein kumulierter xG-Verlauf über die Spielzeit, im Vergleich zu den tatsächlichen Toren.

## 3. Abnahmekriterien

| # | Prüfung | Erwartung |
|---|---|---|
| A-1 | Spiel und Spieler frei wählbar | Daten erscheinen ohne Neustart und ohne Codeänderung |
| A-2 | Torzahl des Spielers | stimmt exakt mit den `Goal`-Events überein |
| A-3 | Schussposition (108, 40), Open Play | xG = 0.24 (± 0.01) |
| A-4 | Schussposition (114, 40), Open Play | xG = 0.65 (± 0.01) |
| A-5 | Elfmeter | xG = 0.78 |
| A-6 | Netzwerk getrennt, Spiel bereits einmal geladen | App funktioniert aus dem Cache |
| A-7 | Spiel ohne Events für den Spieler | verständlicher Leerzustand statt Fehler |

_Zu A-3 bis A-5:_ Prüfen Sie die Berechnung über eine Funktion, die x und y entgegennimmt und xG zurückgibt.

## 4. Abgrenzung (nicht gefordert)
Login, Datenbank, Deployment, CI, Tests über die Abnahmekriterien hinaus.
