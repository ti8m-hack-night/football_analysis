# Requirements – 01 Daten live laden (F-1, Muss)

**User Story:** Als Nutzer möchte ich, dass die App Wettbewerbe, Saisons, Spiele und Events live von StatsBomb lädt, damit keine Daten fest einkompiliert sind.

Quelle und Konstanten: siehe `../_shared/design.md`.

#### Acceptance Criteria
1. WHEN die App startet THEN SHALL sie Wettbewerbe und Saisons live von der Quelle laden.
2. WHEN ein Spiel gewählt wird THEN SHALL die App dessen Events live laden.
3. THE System SHALL keine Spiel-, Event- oder Spielerdaten fest im Code einbetten.
