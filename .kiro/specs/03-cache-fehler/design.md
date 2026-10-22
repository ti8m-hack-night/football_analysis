# Design – 03 Cache und Fehlerrobustheit

Gemeinsame Architektur: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
- Cache-Schicht im Backend (`infrastructure`): Schlüssel = volle StatsBomb-URL, Wert = JSON + Zeitstempel + Herkunft. Server-seitig (In-Memory / lokale Datei-Ablage), keine Datenbank.
- Lesepfad: Cache-Treffer sofort zurück, Herkunft «cache»; sonst HTTP-fetch, bei Erfolg schreiben, Herkunft «live».
- Fehlerpfad: typisierter Fehler (Netz vs. HTTP) im Backend → REST-Fehlerantwort → Fehlerzustand im Frontend (Spec 09); Auswahl bleibt erhalten.
- A-6: zweiter Aufruf bei getrenntem Netz (StatsBomb nicht erreichbar) liefert aus dem Backend-Cache.
