# Workflow

Gilt für die Implementierung aller Specs unter `.kiro/specs/`.

## Branch
- Alle Arbeit auf Branch `owe-hacknight-20261022`.
- Nicht auf `main` committen oder pushen.

## Commits
- Ein Commit pro **abgeschlossener Task** aus einer `tasks.md`.
- Commit-Nachricht nennt Spec und Task, z. B. `07-xg-berechnung: Task 1 – xg()-Funktion`.
- Die Task-Checkbox in `tasks.md` erst nach dem Commit auf `[x]` setzen.

## Definition of Done (je Spec)
Eine Spec gilt erst als fertig, wenn **alle** erfüllt sind:
1. Alle **Muss**-Tasks der Spec umgesetzt und committet.
2. **Tests** für diese Spec erstellt und grün (mindestens die Abnahmekriterien, die die Spec abdeckt).
3. **arc42-Architekturdokumentation** unter `.kiro/docs/arc42/` aktualisiert, soweit die Spec die Architektur berührt (Bausteine, Laufzeit, Entscheidungen).
4. `tasks.md` der Spec vollständig abgehakt.
