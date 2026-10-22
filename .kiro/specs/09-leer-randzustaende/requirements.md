# Requirements – 09 Leer- und Randzustände (A-7, Muss)

**User Story:** Als Nutzer möchte ich bei einem Spieler ohne Aktionen eine klare Rückmeldung, damit ich nicht vor einem Fehler stehe.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. WHEN der gewählte Spieler keine Events im Spiel hat THEN SHALL die App einen verständlichen Leerzustand zeigen statt eines Fehlers. (A-7)
2. WHEN keine gefährlichen Aktionen vorliegen THEN SHALL eine Karte mit Erklärung und Aktion «Spieler wählen» erscheinen.
