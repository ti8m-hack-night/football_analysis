# Design – 08 xG-Modell erweiterbar und Verlauf

Gemeinsame Architektur: siehe [`../_shared/design.md`](../_shared/design.md).

## Delta für diese Spec
- Modell austauschbar (Backend `model`/`service`): `XgModel`-Schnittstelle `xg(shot) -> Double`; Referenzmodell als Default-Implementierung. Zusatzmerkmale (Körperteil, Schussart) in einer alternativen Implementierung.
- Begründungspflicht: Abweichung zur Referenz dokumentieren, wenn Modell gewechselt wird.
- Verlauf (F-17, Frontend): kumulierte Treppenlinien xG berechnet vs. Referenz über 0–90 min, Torpunkte markiert; Daten aus den Aktions-/Stats-Endpunkten.
