# Requirements – 07 xG selbst berechnen (F-13 bis F-15, Muss)

**User Story:** Als Nutzer möchte ich ein eigenes xG je Schuss, damit der Wert nachvollziehbar und nicht nur übernommen ist.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. THE System SHALL xG je Schuss selbst berechnen AND SHALL `shot.statsbomb_xg` nur als Referenz verwenden.
2. THE Modell SHALL sein:
   - Elfmeter (`shot.type.name` = `Penalty`): xG = 0.78.
   - Sonst mit x' = 120 − x:
     - `d = sqrt(x'^2 + (y-40)^2)`
     - `a = atan2(8*x', x'^2 + (y-40)^2 - 16)` (Radiant)
     - `xG = 1 / (1 + e^-(-1.0 - 0.12*d + 2.0*a))`
3. THE System SHALL die xG-Summe je Spieler zeigen AND SHALL sie gegen die Referenzsumme vergleichen (absolut und in %). (F-15)
4. WHEN xG an (108, 40) Open Play berechnet wird THEN SHALL das Ergebnis 0.24 ± 0.01 sein. (A-3)
5. WHEN xG an (114, 40) Open Play berechnet wird THEN SHALL das Ergebnis 0.65 ± 0.01 sein. (A-4)
6. WHEN ein Elfmeter berechnet wird THEN SHALL xG = 0.78 sein. (A-5)
7. THE xG-Berechnung SHALL über eine reine Kotlin-Funktion im `model`-Modul erreichbar sein, die x und y (und `isPenalty`) entgegennimmt und xG zurückgibt — ohne Abhängigkeit zu Framework, Netz oder UI.
