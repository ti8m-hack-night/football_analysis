# Requirements – 05 Linien und Zeitfilter (F-8 Soll, F-9 Kann)

**User Story:** Als Nutzer möchte ich Richtung und Zeitraum der Aktionen sehen, damit ich den Spielverlauf nachvollziehe.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. THE System SHOULD Pässe und Carries als Linie von `location` zu `end_location` zeichnen. (F-8)
2. THE System MAY einen Zeitfilter (Minutenspanne) bieten, der nur Aktionen im Bereich zeigt. (F-9)
3. WHEN der Zeitfilter aktiv ist THEN SHALL die Legenden-Zähler dem Filter folgen.
