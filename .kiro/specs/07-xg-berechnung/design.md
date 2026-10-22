# Design – 07 xG selbst berechnen

Gemeinsame Architektur und xG-Funktion: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
- `xg(x, y, isPenalty)` als reine Kotlin-Funktion im `model`-Modul, separat testbar (keine Abhängigkeit zu Framework/UI/Netz).
- Elfmeter erkannt an `shot.type.name == 'Penalty'`.
- Referenz = `shot.statsbomb_xg`, nie als eigener Wert verwendet.
- Summen je Spieler (im `service`-Modul): Σ berechnet, Σ Referenz, Abweichung absolut und in % = (Σber − Σref) / Σref.
- Prüfwerte (Kotlin-Unit-Tests): (108,40)→0.24±0.01, (114,40)→0.65±0.01, Elfmeter→0.78.
