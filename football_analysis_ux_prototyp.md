# Football Analysis – UX-Prototyp

Analyse eines Fussballspielers in einem Spiel: gefährliche Aktionen auf dem Spielfeld, Statistiken und xG-Berechnung. Dieses Dokument beschreibt den klickbaren UX-Prototyp (Webanwendung, Beispieldaten). Der Prototyp läuft ohne Netz. Teams, Spieler und Werte darin sind erfunden. Das xG-Modell rechnet echt.

Datenquelle im Produkt: [StatsBomb Open Data](https://github.com/statsbomb/open-data). Bei Veröffentlichung ist StatsBomb als Quelle zu nennen, es gelten die Nutzungsbedingungen im Repository.

Prototyp online (privat, Zugriff über das Teilen-Menü): https://claude.ai/artifact/R6VYf3APWVYvb9V7VyoEnX
Mockup online (6 Handy-Bildschirme und Desktop): https://claude.ai/artifact/ELt7Y2FbiFkjiWF7Rmpqc9

---

## 1. Ziel und Umfang

Eine Person wählt Wettbewerb, Saison, Spiel und Spieler und sieht zwei Ansichten:

1. **Aktionen:** alle gefährlichen Aktionen des Spielers auf dem Spielfeld.
2. **Statistik und xG:** Kennzahlen mit Teamvergleich, xG berechnet gegen Referenz, xG-Verlauf.

Im Prototyp enthalten sind alle Muss-Anforderungen als Bedienkonzept, dazu F-8, F-9, F-11 und F-17. Nicht enthalten sind F-12 (Vergleich zweier Spieler) und F-16 (austauschbares Modell).

## 2. Gestaltungsgrundlagen (Apple-Stil)

| Regel | Umsetzung |
| --- | --- |
| Eine Akzentfarbe | Action Blue für Aktionen und Auswahl (hell `#0066cc`, dunkel `#0071e3`, Links `#2997ff`) |
| Pill-Controls | Buttons und Segment-Control sind Pills (Radius 999) |
| Keine Schatten | Flächenwechsel und Haarlinien (`rgba(0,0,0,.12)`) statt Schatten |
| Schrift | Systemschrift (`-apple-system`, SF Pro), Gewichte 400, 600, 700, nie 500 |
| Zahlen | tabellarische Ziffern in Tabellen und Kennzahlen |
| Themen | Hell und Dunkel über Farbtoken, Dunkel folgt dem System |
| Ecken | Karten und Auswahlzeilen 16 bis 20 px, Eingabefelder 12 px |

Datenfarben der Aktionstypen (zusätzlich zur Form, nie allein):

| Aktionstyp | Farbe hell | Farbe dunkel |
| --- | --- | --- |
| Tor | `#248a3d` | `#30d158` |
| Schuss | `#0066cc` | `#2997ff` |
| Schlüsselpass | `#c76a00` | `#ff9f0a` |
| Dribbling im Strafraum | `#8944ab` | `#bf5af2` |
| Carry in den Strafraum | `#0a7f95` | `#64d2ff` |

## 3. Ablauf

```
Start
  └─ Wettbewerbe laden (Standard: FIFA World Cup 2022)
       └─ Saison wählen → Spiele laden (Standard: letztes Spiel)
            └─ Spiel wählen → Events laden
                 └─ Spieler wählen (nur Spieler mit Events)
                      ├─ Tab «Aktionen»
                      └─ Tab «Statistik & xG»
```

- Standardspieler ist der mit den meisten gefährlichen Aktionen.
- Die Spielerwahl wirkt sofort, Wettbewerb, Saison und Spiel laden Daten nach (kurzer Ladezustand).
- Die Auswahl öffnet sich als Sheet (Handy: von unten, Desktop: zentriert). Schliessen mit «Fertig», Esc oder Klick auf den abgedunkelten Hintergrund.

## 4. Bildschirme

### 4.1 Kopfbereich und Auswahlzeile

```
┌───────────────────────────────────────────────┐
│ Football Analysis            [ Auswahl ändern ]│  ← Leiste, bleibt oben
├───────────────────────────────────────────────┤
│ StatsBomb Open Data                            │
│ Football Analysis                      (gross) │
│ Gefährliche Aktionen, Statistiken und xG …     │
│                                                │
│ ┌────────────────────────────────────────────┐ │
│ │ BEISPIEL-WELTMEISTERSCHAFT 2022            │ │
│ │ Team Blau 2 : 1 Team Rot             Ändern ›│ │
│ │ Finale · 18.12.2022                        │ │
│ └────────────────────────────────────────────┘ │
│ ● Live von StatsBomb geladen und im Cache …    │
│ ───────────────────────────────────────────── │
│ Beispielspieler 10   [Team Blau]               │
│ 15 gefährliche Aktionen in diesem Spiel        │
│ (Aktionen | Statistik & xG)                    │
└───────────────────────────────────────────────┘
```

Der Titel in der Leiste erscheint erst, wenn der grosse Titel aus dem Bild gescrollt ist. Die Zeile «Live von StatsBomb geladen …» zeigt die Herkunft der Daten (Live oder Cache).

### 4.2 Aktionen

```
 ◎ Tor 2   ● Schuss 5   ■ Schlüsselpass 3   □ Dribbling im Strafraum 2   ○··● Carry 3
 Von Minute (●────────) 0        Bis Minute (────────●) 90

 ┌──────────────────────────────────────┐   Min  Aktion         Ergebnis       xG
 │ Spielfeld 120 × 80, Angriff nach rechts│   8′   ■ Schlüsselpass Vorlage zum Schuss
 │                          ┌───────┐   │   12′  ● Schuss        Gehalten      0.11
 │        ○···●             │ ◎  ●  │   │   15′  ○ Carry         Von ausserhalb …
 │                          └───────┘   │   23′  ◎ Tor           Tor           0.25
 └──────────────────────────────────────┘   …
```

- **Legende** mit Zählern. Die Zähler folgen dem Zeitfilter.
- **Zeitfilter:** zwei Regler (Von, Bis) in Minuten, 0 bis 90 (im Produkt bis Spielende inkl. Verlängerung).
- **Spielfeld und Liste** sind verknüpft: Überfahren einer Zeile markiert die Aktion im Spielfeld.
- **Tooltip** am Spielfeld (Hover, Fokus, Tippen): Minute, Aktionstyp, Ergebnis, bei Schüssen xG berechnet und Referenz, z. B. «23′ · Tor, xG 0.25 (Referenz 0.19)».
- Auf dem Desktop stehen Spielfeld (2/3) und Liste (1/3) nebeneinander, auf dem Handy untereinander.

### 4.3 Statistik und xG

```
 xG im Vergleich
 ┌──────────────┬──────────────┬──────────────┐
 │ 1.95         │ 1.77         │ +0.18        │
 │ xG berechnet │ xG Referenz  │ Abweichung: +10.1 %
 └──────────────┴──────────────┴──────────────┘

 Kennzahlen                  Beispielspieler 10     Ø Team pro Spieler (3)
 Schüsse                          7                  …
 Schüsse aufs Tor                 5
 Tore                             2
 Pässe / Angekommene Pässe        41 / 33
 Passquote                        80 %               …
 Schlüsselpässe                   3
 Erfolgreiche Dribblings          5
 xG (berechnet) / (Referenz)      1.95 / 1.77

 Kumulierter xG-Verlauf   ─ xG berechnet   ─ xG Referenz   ─ Tore
 (Treppenlinien über 0 bis 90 Minuten, Punkte bei Toren)
```

Der Teamdurchschnitt gilt pro Spieler über alle Spieler des Teams mit Ereignissen im Spiel. Die Passquote des Teams ist die Summe aller Pässe.

## 5. Zustände

| Zustand | Darstellung | Nächste Handlung |
| --- | --- | --- |
| **Laden** | Platzhalterflächen für Auswahlzeile, Tabs, Spielfeld und Zeilen, dazu Spinner mit Text, Auswahl gesperrt | Warten |
| **Fehler** | Rotes Banner: «Keine Verbindung zu StatsBomb. Prüfe die Internetverbindung und versuche es erneut. Deine Auswahl bleibt erhalten.» | «Erneut versuchen» oder «Zuletzt geladenes Spiel aus dem Cache öffnen» |
| **Offline** | Inhalt normal, dazu Hinweis «Offline. Dieses Spiel wurde schon einmal geladen und kommt aus dem Cache.» | – |
| **Leer** | Karte «Keine gefährlichen Aktionen» mit Erklärung und Button «Spieler wählen» | Anderen Spieler wählen |

Im Prototyp schaltet eine Leiste oben zwischen Normal, Lädt, Fehler, Offline und Leer um. Der Leerzustand entsteht auch durch die Wahl von «Beispielspieler 1 (Torwart)».

## 6. Spielfeld

- Koordinaten wie StatsBomb: 120 × 80, Ursprung oben links, Angriffsrichtung immer nach rechts, Tor bei x = 120, y = 36 bis 44.
- Strafraum: x ≥ 102, y 18 bis 62. Im Spielfeld getönt, damit das Kriterium «im Strafraum» sichtbar ist.
- Markierungen: Aussenlinie, Mittellinie, Mittelkreis, beide Strafräume, beide Torräume (x 0 bis 6 und 114 bis 120, y 30 bis 50), Tore, Elfmeterpunkte (x 12 und 108), Halbkreise vor den Strafräumen.
- Alle Markierungen bestehen aus Rechtecken und Kreisen, damit sie in jedem Renderer erscheinen.

### Aktionstypen und Symbole

| Typ | Kriterium (F-5) | Symbol | Darstellung |
| --- | --- | --- | --- |
| Schuss | jeder Schuss | gefüllter Kreis | Grösse = berechnetes xG |
| Tor | Schuss mit Ergebnis Goal | gefüllter Kreis mit Ring | hervorgehoben |
| Schlüsselpass | Pass mit `shot_assist` oder `goal_assist` | gefülltes Quadrat | punktierte Linie zum Endpunkt, grösserer Endpunkt |
| Dribbling im Strafraum | Dribble mit Ergebnis Complete im Strafraum | hohles Quadrat | – |
| Carry in den Strafraum | beginnt ausserhalb, endet im Strafraum | hohler Kreis | punktierte Linie zum Endpunkt, grösserer Endpunkt |

Das Elfmeterschiessen (Periode 5) erscheint als «n. E.» und zählt bei Toren und xG mit.

## 7. xG-Berechnung

Elfmeter: xG = 0.78. Alle anderen Schüsse (Position x, y; x′ = 120 − x):

```
d  = sqrt(x′² + (y − 40)²)
a  = atan2(8·x′, x′² + (y − 40)² − 16)        (Radiant)
xG = 1 / (1 + e^−(−1.0 − 0.12·d + 2.0·a))
```

Die Summe wird pro Spieler berechnet und mit der Referenzsumme (`shot.statsbomb_xg`) verglichen, absolut und in Prozent.

Prüfwerte (Fusszeile des Prototyps, «Modellprüfung xG»):

| Fall | Ergebnis | Erwartet |
| --- | --- | --- |
| Schuss (108, 40), Open Play | 0.240 | 0.24 ± 0.01 |
| Schuss (114, 40), Open Play | 0.653 | 0.65 ± 0.01 |
| Elfmeter | 0.78 | 0.78 |

## 8. Kennzahlen (F-10)

Schüsse, Schüsse aufs Tor (Goal, Saved, Saved to Post), Tore, Pässe, angekommene Pässe (Pass ohne `outcome`), Passquote in %, Schlüsselpässe, erfolgreiche Dribblings (alle, nicht nur im Strafraum), xG berechnet, xG Referenz. Die Torzahl entspricht exakt den Goal-Events des Spielers.

## 9. Beispieldaten im Prototyp (erfunden)

Wettbewerbe: «Beispiel-Weltmeisterschaft» (Saisons 2022, 2018), «Beispiel-Liga» (2023/24). Spiele: Team Blau 2:1 Team Rot (Finale, 18.12.2022) und Team Gelb 0:0 Team Grün (Halbfinale, 14.12.2022).

Spieler: Beispielspieler 10, 7 und 1 (Torwart, Team Blau), 9 (Team Rot), 5 (Team Gelb), 8 (Team Grün).

Schüsse von Beispielspieler 10:

| Minute | Position | Ergebnis | xG berechnet | xG Referenz |
| --- | --- | --- | --- | --- |
| 12′ | (104, 36) | Gehalten | 0.11 | 0.12 |
| 23′ | (110, 45) | Tor | 0.25 | 0.19 |
| 34′ | (96, 28) | Neben das Tor | 0.02 | 0.04 |
| 51′ | (108, 40) Elfmeter | Tor | 0.78 | 0.78 |
| 63′ | (114, 41) | Gehalten | 0.64 | 0.52 |
| 78′ | (101, 52) | Geblockt | 0.04 | 0.05 |
| 88′ | (106, 30) | Gehalten | 0.09 | 0.07 |
| **Summe** | | | **1.95** | **1.77** (+0.18, +10.1 %) |

Dazu kommen drei Schlüsselpässe (8′, 44′ als Torvorlage, 70′), zwei Dribblings im Strafraum (29′, 57′) und drei Carries in den Strafraum (15′, 41′, 66′).

## 10. Abdeckung der Anforderungen

| Anforderung | Priorität | Im Prototyp |
| --- | --- | --- |
| F-1 bis F-3 | Muss | Auswahl im Sheet, Hinweis Live oder Cache, Ladezustand, Fehler mit Wiederholen, Offline aus Cache |
| F-4 bis F-7 | Muss | Spielfeld, unterscheidbare Aktionstypen mit Legende, Tore hervorgehoben, Tooltip und Liste |
| F-8 | Soll | Pässe und Carries als Linie vom Start- zum Endpunkt |
| F-9 | Kann | Zeitfilter mit zwei Reglern |
| F-10 | Muss | Kennzahlentabelle |
| F-11 | Soll | Spalte «Ø Team pro Spieler» |
| F-13 bis F-15 | Muss | xG pro Schuss berechnet, Summe gegen Referenz mit Abweichung |
| F-17 | Kann | Kumulierter xG-Verlauf mit Toren |
| F-12, F-16 | Kann, Soll | Nicht im Prototyp |
| A-7 | Abnahme | Leerzustand für Spieler ohne gefährliche Aktionen |

## 11. Bedienung und Barrierefreiheit

- Alle Auswahlfelder sind beschriftet, Tabs haben die Rollen `tablist` und `tab`.
- Spielfeld-Markierungen sind per Tastatur fokussierbar und haben einen Text mit Minute, Typ und Ergebnis.
- Farbe ist nie das einzige Merkmal: jede Aktion hat eine eigene Form.
- Bewegung (Ladeanimation) wird bei «Bewegung reduzieren» abgeschaltet.
- Das Layout funktioniert ab etwa 400 px Breite ohne seitliches Scrollen.
