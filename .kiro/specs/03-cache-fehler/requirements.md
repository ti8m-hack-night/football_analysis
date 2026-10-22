# Requirements – 03 Cache und Fehlerrobustheit (F-3, Muss)

**User Story:** Als Nutzer möchte ich, dass geladene Daten lokal zwischengespeichert werden, damit die App ohne Netz weiterarbeitet und Fehler verständlich bleiben.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. WHEN eine Antwort geladen wurde THEN SHALL die App sie lokal zwischenspeichern.
2. WHEN das Netz getrennt ist AND das Spiel bereits einmal geladen wurde THEN SHALL die App aus dem Cache funktionieren. (A-6)
3. WHEN ein Ladefehler auftritt THEN SHALL die App eine verständliche Meldung zeigen AND SHALL NOT abstürzen.
