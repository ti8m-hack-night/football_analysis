# Design – 09 Leer- und Randzustände

Gemeinsame Architektur und Zustände: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
UI-Zustände vollständig:
- `Laden`: Platzhalter + Spinner, Auswahl gesperrt.
- `Fehler`: rotes Banner, «Erneut versuchen» / «aus Cache öffnen», Auswahl bleibt.
- `Offline`: Inhalt + Hinweis «aus dem Cache».
- `Leer` (A-7): Karte «Keine gefährlichen Aktionen» mit Erklärung und Button «Spieler wählen». Auch bei Torwart/Spieler ohne Events.
