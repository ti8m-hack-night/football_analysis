# Requirements – 06 Ansicht «Statistiken» (F-10 Muss, F-11 Soll, F-12 Kann)

**User Story:** Als Nutzer möchte ich die Kennzahlen des Spielers als Tabelle sehen, damit ich seine Leistung quantitativ bewerte.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. THE System SHALL eine Tabelle zeigen mit: Schüsse, Schüsse aufs Tor (`Goal`, `Saved`, `Saved to Post`), Tore, Pässe, angekommene Pässe (Pass ohne `pass.outcome`), Passquote %, Schlüsselpässe (`shot_assist` oder `goal_assist`), erfolgreiche Dribblings, xG (berechnet), xG (Referenz).
2. THE Torzahl SHALL exakt den `Goal`-Events des Spielers entsprechen. (A-2)
3. THE System SHOULD den Spieler mit dem Teamdurchschnitt pro Spieler des Spiels vergleichen. (F-11)
4. THE System MAY zwei Spieler nebeneinander vergleichen. (F-12)
