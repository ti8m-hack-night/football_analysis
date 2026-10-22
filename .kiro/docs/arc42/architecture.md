# Architektur – Football Analysis (arc42)

Status: Skelett. Wird je Spec gemäss Definition of Done (`.kiro/steering/workflow.md`) aktualisiert.
Basis: arc42-Template. Specs: `.kiro/specs/`, geteiltes Design: `.kiro/specs/_shared/design.md`.

## 1. Einführung und Ziele
Analyse eines Fussballspielers in einem Spiel: gefährliche Aktionen auf dem Spielfeld, Kennzahlen und selbst berechnetes xG. Lokal lauffähig, Daten live von StatsBomb.

Qualitätsziele:
1. Korrektheit der xG-Berechnung (Abnahmekriterien A-3 bis A-5).
2. Robustheit ohne Netz (Cache, A-6) und bei Leer-/Fehlerzuständen (A-7).
3. Nachvollziehbarkeit der Kennzahlen (Torzahl == Goal-Events, A-2).

## 2. Randbedingungen
Siehe `.kiro/steering/tech.md`. Lokal, live-Daten, kein Server/DB/Login/Deploy; StatsBomb-Attribution; Tech frei.

## 3. Kontextabgrenzung
```
[Nutzer] ── wählt Wettbewerb/Saison/Spiel/Spieler ──> [Football Analysis (lokal)]
[Football Analysis] ── HTTPS fetch ──> [StatsBomb Open Data]
```
Einzige externe Schnittstelle: StatsBomb Open Data (read-only JSON).

## 4. Lösungsstrategie
Client-only. Dienste: StatsBombClient (fetch+Cache), EventClassifier, StatsCalculator, XgModel (reine Funktion). Details: `_shared/design.md`.

## 5. Bausteinsicht
_Zu füllen je Spec._ Oberste Ebene: UI · State · Dienste · Cache. Ausgangspunkt: Diagramm in `_shared/design.md`.

## 6. Laufzeitsicht
_Zu füllen je Spec._ Hauptablauf: competitions → matches → events → Spielerliste → Klassifikation + Kennzahlen + xG → Ansichten.

## 7. Verteilungssicht
Ein Artefakt (statische Web-App), läuft im Browser des Nutzers. Keine Server-Verteilung.

## 8. Querschnittliche Konzepte
_Zu füllen je Spec._ Caching, Fehlerbehandlung, Barrierefreiheit, Theming (hell/dunkel), xG-Modell. Siehe `_shared/design.md`.

## 9. Architekturentscheidungen (ADR)
_Zu füllen je Spec._ Kandidaten: xG als reine Funktion; Cache IndexedDB vs. localStorage; kein Framework.

## 10. Qualitätsanforderungen
Abnahmekriterien A-1 bis A-7 als Qualitätsszenarien. Siehe Specs.

## 11. Risiken und technische Schulden
_Zu füllen._ Offen: Verzeichnisstruktur (Repo-Root vs. `FootballAnalysis/`-Unterordner) noch zu klären.

## 12. Glossar
- **xG**: Expected Goals, Torwahrscheinlichkeit eines Schusses.
- **Strafraum**: x ≥ 102, 18 ≤ y ≤ 62 (StatsBomb-Koordinaten).
- **Gefährliche Aktion**: Schuss, Schlüsselpass, Dribbling im Strafraum, Carry in den Strafraum (F-5).
