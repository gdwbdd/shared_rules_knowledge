# Workbasis-Struktur eines Projekts

> Wie ein Projekt seine Arbeitsbasis für KI-Assistenten aufbaut und dieses Repo einbindet.
> Vorlagen liegen unter `templates/`. Vorbild und erste Umsetzung: `mec_demo` (2026-07 bis 2026-09).

## Schichten und Pflegeorte

| Schicht | Ort | Enthält | Gilt für |
|---|---|---|---|
| geteilt | dieses Repo, als Submodul `docs/shared_rules_knowledge/` | generischer Codex, Lernprozesse, ADR-Prozess, Domänenwissen, Werkzeugumgebung | alle Projekte |
| Projekt | `docs/workbasis/` im Projekt-Repo | Projekt-Codex, open_points, chat_log, knowledge (projektspezifisch), adr (Projektentscheidungen), Terminologie (Projektbegriffe) | dieses Projekt |
| Einstieg | `CLAUDE.md`, `.github/copilot-instructions.md` | nur Leseordnung und Importe, kein eigener Regeltext | Assistent |
| lokal | `.claude/settings.local.json`, Memory-Ordner | Berechtigungen, Zeiger | dieser Rechner |

Regel: Jeder Inhalt hat genau einen Ort. Kein Regeltext in `CLAUDE.md` oder
`copilot-instructions.md`, keine Kopien geteilter Regeln im Projekt-Codex.

## Dateien im Projekt (`docs/workbasis/`)

- `codex.md` — nur Projektspezifisches: Architektur-Regeln, Datenstrukturen, Ports, Start-
  und Neustart-Befehle, Projekt-Kontext, Abweichungen von den geteilten Regeln (ausdrücklich
  markiert). Erste Zeile verweist auf den geteilten Codex.
- `open_points.md` — offene Punkte und Wiedervorlage. Session-Start ZUERST lesen, Session-Ende
  ZULETZT aktualisieren. Handoff-Pflicht: jede Zusage "speichern & später vorlegen" sofort hier.
- `chat_log.md` — Chatverlauf und Kontext je Datum, selbstständig gepflegt.
- `knowledge/INDEX.md` — verifizierte Umgebungsfakten, Architektur-Stichworte, Links auf
  Themen-Dateien. Generisches gehört ins geteilte `knowledge/`, nicht hierher.
- `knowledge/terminologie.md` — Projektglossar: nur Begriffe dieses Projekts, verweist für
  Domänenbegriffe und Invarianten auf das geteilte Glossar.
- `adr/` — Projektentscheidungen (README mit Verzeichnis; Prozess und Vorlage kommen aus dem
  geteilten `adr/`).
- `PROJECT_PLAN.md` (Repo-Wurzel) — Weiterentwicklung und Umsetzung.

## Leseordnung beim Session-Start (Assistent)

1. geteilt: `rules/codex.md`, `rules/lernprozesse.md` (werden per `@` in `CLAUDE.md` importiert)
2. Projekt: `docs/workbasis/codex.md`
3. Projekt: `docs/workbasis/open_points.md`
4. Projekt: `docs/workbasis/chat_log.md`
5. Projekt: `docs/workbasis/knowledge/INDEX.md`, dann `knowledge/terminologie.md`
6. geteilt: `knowledge/terminologie.md`, `knowledge/INDEX.md` bei Bedarf
7. Projekt: `docs/workbasis/adr/README.md`

Beim ersten Kontakt eines neuen Tages: 1, 2, 5, 7 erneut lesen (Tagesstart-Regel).

## Neues Projekt aufsetzen

1. Submodul einbinden (siehe `README.md` dieses Repos).
2. `templates/CLAUDE.md` → `CLAUDE.md`; `templates/copilot-instructions.md` →
   `.github/copilot-instructions.md`.
3. `templates/codex_projekt.md` → `docs/workbasis/codex.md`, Projektteil ausfüllen.
4. `templates/open_points.md`, `templates/chat_log.md`, `templates/knowledge_INDEX.md`,
   `templates/terminologie_projekt.md` an ihre Orte kopieren.
5. `docs/workbasis/adr/README.md` mit leerem Verzeichnis anlegen (Vorlage in
   `templates/adr_README_projekt.md`).
6. Abschnitt "Klonen (Submodul erforderlich)" aus `templates/README_abschnitt_klonen.md` in die
   Projekt-`README.md` übernehmen (Pflicht: sonst klonen Menschen ohne Submodul).
7. Alles committen und pushen; ab jetzt gehört `docs/workbasis/` in jeden Commit, der es ändert.

Checkliste "ist ein Projekt vollständig eingebunden?": `.gitmodules` vorhanden, `CLAUDE.md` mit
zwei `@`-Importen und Leer-Hinweis, `copilot-instructions.md` mit Leseordnung und Leer-Hinweis,
`README.md` mit Klon-Abschnitt, Projekt-Codex ohne generische Regeln.

## Geteilten Stand nachziehen

```
git submodule update --remote docs/shared_rules_knowledge
git add docs/shared_rules_knowledge
git commit -m "shared_rules_knowledge auf <kurz-hash> nachgezogen"
git push
```
Nur auf Wort des Users (Commit-Regel). Der Assistent meldet, wenn der Submodul-Stand hinter
`origin/main` des geteilten Repos liegt.

## Was wohin bei neuen Erkenntnissen

| Erkenntnis | Ort |
|---|---|
| Regel der Zusammenarbeit, gilt überall | geteilt `rules/codex.md` (+ Fall in `lernprozesse.md`), nach Go des Users |
| Lektion mit Datum und Fall | geteilt `rules/lernprozesse.md` (ohne Nachfrage, Regel Persistenz) |
| Nova-/Isaac-/Werkzeug-Fakt, projektunabhängig | geteilt `knowledge/...`, markiert verifiziert/Annahme |
| Projektfakt (Pfad, Port, Prim, Pose, Datenstruktur) | Projekt `knowledge/INDEX.md` oder Projekt-Codex |
| Entscheidung über Projektcode | Projekt `adr/NNN-...md` |
| Entscheidung über den geteilten Rahmen | geteilt `adr/NNN-...md` |
| Zusage, offener Punkt | Projekt `open_points.md` |
