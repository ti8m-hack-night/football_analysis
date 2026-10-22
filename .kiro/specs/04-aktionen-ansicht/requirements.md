# Requirements – 04 Ansicht «Gefährliche Aktionen» (F-4 bis F-7, Muss)

**User Story:** Als Nutzer möchte ich die gefährlichen Aktionen des Spielers auf einem Spielfeld sehen, damit ich sein Gefahrenpotenzial räumlich erfasse.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. THE System SHALL ein Spielfeld mit allen gefährlichen Aktionen des Spielers an ihren Positionen zeigen.
2. THE System SHALL als gefährlich werten:
   - jeden Schuss,
   - jeden Pass mit `pass.shot_assist` oder `pass.goal_assist`,
   - jedes `Dribble` mit `outcome` `Complete`, das im Strafraum stattfindet,
   - jeden `Carry`, der ausserhalb des Strafraums beginnt und im Strafraum endet.
3. THE System SHALL Aktionstypen durch Farbe und Form unterscheidbar machen AND SHALL eine Legende zeigen AND SHALL Tore hervorheben.
4. WHEN eine Aktion fokussiert oder überfahren wird THEN SHALL ein Tooltip Minute, Aktionstyp und Ergebnis zeigen, bei Schüssen zusätzlich xG (berechnet und Referenz).
