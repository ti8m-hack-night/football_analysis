# Geteiltes Design – Football Analysis

Gemeinsame Architektur für alle Specs. Jede Spec verweist hierauf (`../_shared/design.md`) und ergänzt nur ihren eigenen Teil.

## Überblick

Quarkus-Backend (Kotlin) lädt StatsBomb Open Data live, cached sie, klassifiziert gefährliche Aktionen, rechnet Kennzahlen und xG. Ein React-Frontend holt fertige Ergebnisse per REST und zeigt zwei Ansichten für einen Spieler: **Aktionen** und **Statistik & xG**. Keine Spiel-, Event- oder Spielerdaten sind fest im Code eingebettet.

- **Backend:** Quarkus + Kotlin. Lädt, cached, klassifiziert, rechnet.
- **Frontend:** React. Wählt aus, zeigt Spielfeld und Tabelle, hält UI-Zustände.
- **Aufteilung:** Das Backend rechnet; das Frontend rendert. React fragt REST-Endpunkte, keine Domänenlogik im Browser.

## Architektur

```
┌───────────────────────── React (Frontend) ──────────────────────────┐
│ Ansichten (Aktionen, Statistik), Auswahl-Sheet, UI-Zustände          │
│ ruft REST ↓                                                          │
└──────────────────────────────┬───────────────────────────────────────┘
                               │ REST (JSON)
┌──────────────────────────────▼──────────── Quarkus + Kotlin ─────────┐
│ api             REST-Ressourcen (Resource/Controller), DTOs           │
│ service         Anwendungslogik: EventClassifier, StatsCalculator,    │
│                 XgModel (Orchestrierung der Analyse)                   │
│ infrastructure  StatsBombClient (HTTP zu StatsBomb), Cache            │
│ model           Domänentypen: DangerousAction, PlayerStats, Shot …    │
└──────────────────────────────┬───────────────────────────────────────┘
                               │ HTTP fetch
                               ▼
         StatsBomb Open Data (raw.githubusercontent.com)
```

Datenfluss: `competitions.json` → Saison → `matches/{c}/{s}.json` → Spiel → `events/{match}.json` → Spielerliste (aus Events) → Klassifikation + Kennzahlen + xG (Backend) → REST → Ansichten (React).

## Module (Onion-Architektur, ArchUnit-geprüft)

Vier Module, Abhängigkeiten strikt nach innen:

```
api  ──►  service  ──►  model
              │
infrastructure ──► model
```

| Modul            | Inhalt                                                             | Darf abhängen von   |
| ---------------- | ------------------------------------------------------------------ | ------------------- |
| `model`          | Domänentypen, reine Werte, xG-Funktion. Kein Framework.            | – (nichts)          |
| `service`        | Anwendungsfälle: Klassifikation, Kennzahlen, xG-Orchestrierung.    | `model`             |
| `infrastructure` | StatsBomb-HTTP-Client, Cache. Setzt Ports aus `service`/`model` um. | `model`, `service`  |
| `api`            | REST-Ressourcen, DTOs, Mapping.                                     | `service`, `model`  |

- `model` hängt von nichts ab (innerste Schicht, kein Quarkus).
- Ports (Interfaces) liegen innen (`service`/`model`), Adapter (StatsBomb, Cache) in `infrastructure`.
- **ArchUnit-Test** erzwingt diese Regeln (siehe Teststrategie). Jede Verletzung bricht den Build.

## Quelle und Konstanten

- Basis-URL: `https://raw.githubusercontent.com/statsbomb/open-data/master/data/`
- Einstieg: FIFA World Cup 2022 (`competition_id` 43, `season_id` 106).
- Spielfeld 120 × 80, Angriff nach rechts, Tor x = 120 / y 36–44.
- Strafraum-Test: `x >= 102 && y >= 18 && y <= 62`.
- Auflage: StatsBomb als Quelle nennen.

## Dienste (gemeinsame Verträge)

### StatsBombClient  (`infrastructure`)
`getCompetitions()`, `getMatches(c, s)`, `getEvents(matchId)`, `getLineups(matchId)`. Pro Methode: Cache-Lookup je URL → sonst HTTP-`fetch` → bei Erfolg cachen. Typisierte Fehler (Netz vs. HTTP), Herkunft «live/cache» mitführen.

### Cache  (`infrastructure`)
Schlüssel = volle URL, Wert = JSON + Zeitstempel + Herkunft. Server-seitiger Cache (In-Memory / lokale Datei-Ablage, keine Datenbank). Hält geladene Antworten vor, damit ein zweiter Aufruf ohne Netz aus dem Cache bedient wird.

### EventClassifier  (`service`)
Eingabe: Events + Spieler-ID → `DangerousAction { type, minute, location, endLocation?, outcome, xg?, xgRef? }`. Regeln siehe Spec 04/05.

### StatsCalculator  (`service`)
Kennzahlen je Spieler + Teamdurchschnitt. Regeln siehe Spec 06.

### XgModel (reine Funktion, `model`)
```kotlin
fun xg(x: Double, y: Double, isPenalty: Boolean): Double {
    if (isPenalty) return 0.78
    val xp = 120 - x
    val dy = y - 40
    val d = Math.sqrt(xp * xp + dy * dy)
    val a = Math.atan2(8 * xp, xp * xp + dy * dy - 16)
    return 1.0 / (1.0 + Math.exp(-(-1.0 - 0.12 * d + 2.0 * a)))
}
```
Reine Funktion im `model`-Modul, keine Abhängigkeit zu Framework/Netz/UI. Details und Prüfwerte siehe Spec 07.

## REST-Schnittstelle (grob)

Das Frontend ruft nur das Backend, nie StatsBomb direkt (löst auch CORS). REST folgt den [Zalando RESTful API Guidelines](https://github.com/zalando/restful-api-guidelines): Kebab-Case in Pfaden, `snake_case` in Query- und Pfad-Parametern sowie JSON-Feldern, Pluralnamen für Collections, keine Verben in Pfaden, Problem-JSON (RFC 9457) für Fehler. Geplante Ressourcen:

- `GET /api/competitions` → Wettbewerbe + Saisons.
- `GET /api/matches?competition_id={id}&season_id={id}` → Spiele.
- `GET /api/matches/{match_id}/players` → Spieler mit Events.
- `GET /api/matches/{match_id}/players/{player_id}/dangerous-actions` → gefährliche Aktionen inkl. xG.
- `GET /api/matches/{match_id}/players/{player_id}/stats` → Kennzahlen inkl. xG-Summen.

Fehlerantworten als `application/problem+json` (RFC 9457) mit `type`, `title`, `status`, `detail`.

## Gestaltung (gemeinsam, Frontend)

Apple-Stil: eine Akzentfarbe (Action Blue, hell `#0066cc` / dunkel `#0071e3`), Pill-Controls (Radius 999), keine Schatten (Haarlinien `rgba(0,0,0,.12)`), Systemschrift (400/600/700), tabellarische Ziffern, Hell/Dunkel über Token (Dunkel folgt System), Ecken 16–20 px.

Datenfarben (hell/dunkel): Tor `#248a3d`/`#30d158`, Schuss `#0066cc`/`#2997ff`, Schlüsselpass `#c76a00`/`#ff9f0a`, Dribbling `#8944ab`/`#bf5af2`, Carry `#0a7f95`/`#64d2ff`. Farbe nie allein — immer auch Form.

## Fehlerbehandlung (gemeinsam)
Netz-/HTTP-Fehler im Backend → typisierter Fehler → REST-Fehlerantwort; Frontend zeigt Fehlerzustand, Auswahl bleibt erhalten, Cache-Öffnen anbieten. Fehlende Event-Felder → defensiv überspringen. Details Spec 03/09.

## Teststrategie (gemeinsam, nur bis Abnahmekriterien + Architektur)
- Unit-Tests nur für die Abnahmekriterien A-1 bis A-7, je in der zuständigen Spec.
- **ArchUnit-Test** (Pflicht, kein Abnahmekriterium): erzwingt die Onion-Abhängigkeiten der vier Module. Dieser Test ist Teil der Definition of Done, sobald Backend-Module existieren.
- Darüber hinaus keine Tests (Abgrenzung).
