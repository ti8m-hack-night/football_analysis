# Design – 05 Linien und Zeitfilter

Gemeinsame Architektur: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
Reine Frontend-Visualisierung; `DangerousAction` trägt `endLocation` bereits aus dem Backend.
- Linien (F-8): punktierte Linie vom Start- zum Endpunkt für Schlüsselpass und Carry; Endpunkt grösser.
- Zeitfilter (F-9): zwei Regler Von/Bis in Minuten, 0–90 (Produkt bis Spielende inkl. Verlängerung). Filter wirkt im Frontend auf Spielfeld, Liste und Legenden-Zähler.
