# shared_rules_knowledge

Single Source of Truth (SSoT) für alles, was projektübergreifend gilt, wenn Georg Dude mit
KI-Assistenten (Claude Code, GitHub Copilot) arbeitet: Regeln der Zusammenarbeit, gelernte
Lektionen, Arbeitsstruktur, Domänenwissen zu Wandelbots Nova und Isaac Sim, Fakten zur
Werkzeugumgebung.

**Grundsatz:** Ein Inhalt hat genau einen Pflegeort. Steht etwas hier, wird es in keinem
Projekt kopiert, sondern referenziert. Projekte pflegen nur, was nur sie betrifft.

Entstanden 2026-09-11 aus `mec_demo` (`docs/workbasis/`), den Memory-Ordnern zweier Projekte
und `~/.claude/frame_settings.md`. Begründung und verworfene Alternativen: [adr/001](adr/001-geteilte-regeln-als-ssot-per-submodul.md).

## Struktur

| Pfad | Inhalt |
|---|---|
| [rules/codex.md](rules/codex.md) | Generische Regeln der Zusammenarbeit (Sprache, Wortlaut, Freigabe, Kommunikation, Konsistenz, ADR, Workflow) |
| [rules/lernprozesse.md](rules/lernprozesse.md) | Gelernte Lektionen mit Datum, Fall und abgeleiteter Regel |
| [rules/workbasis_struktur.md](rules/workbasis_struktur.md) | Wie ein Projekt seine `docs/workbasis/` aufbaut und dieses Repo einbindet |
| [adr/](adr/README.md) | ADR-Prozess (README, TEMPLATE) und die ADRs dieses Repos |
| [knowledge/INDEX.md](knowledge/INDEX.md) | Domänenwissen: Nova SDK, Isaac OmniService, Terminologie, novaflow-Referenz, Claude-Code-Umgebung |
| [templates/](templates/) | Vorlagen für neue Projekte (CLAUDE.md, copilot-instructions, Projekt-Codex, open_points, chat_log, INDEX, Terminologie) |

## Einbinden in ein Projekt

1. Submodul anlegen (Pfad ist Konvention, selbsterklärend):
   ```
   git submodule add https://github.com/gdwbdd/shared_rules_knowledge.git docs/shared_rules_knowledge
   ```
2. `CLAUDE.md` des Projekts importiert die geteilten Regeln (Vorlage: `templates/CLAUDE.md`):
   ```
   @docs/shared_rules_knowledge/rules/codex.md
   @docs/shared_rules_knowledge/rules/lernprozesse.md
   ```
3. `.github/copilot-instructions.md` verweist auf dieselben Dateien (Vorlage in `templates/`).
4. Projekt-Codex (`docs/workbasis/codex.md`) enthält nur noch Projektspezifisches und verweist
   nach oben (Vorlage: `templates/codex_projekt.md`).
5. `README.md` des Projekts bekommt den Abschnitt "Klonen (Submodul erforderlich)" aus
   `templates/README_abschnitt_klonen.md`, damit auch Menschen ohne Assistent wissen, dass
   `git clone --recurse-submodules` bzw. `git submodule update --init` nötig ist.

## Klonen eines eingebundenen Projekts

Ein normales `git clone` lässt `docs/shared_rules_knowledge/` **leer**; die `@`-Importe in
`CLAUDE.md` laufen dann ins Leere und Copilot findet die Regeln nicht.

```
git clone --recurse-submodules <projekt-url>      # neu
git submodule update --init                       # nachträglich
```

## Pflege

- **Ändern nur hier.** Regeländerungen, neue Lektionen, neue Domänenfakten werden in diesem
  Repo committet und gepusht.
- Projekte holen den neuen Stand mit `git submodule update --remote docs/shared_rules_knowledge`
  und committen den neuen Submodul-Zeiger. Der Zeiger pinnt bewusst einen Stand; ein Projekt
  entscheidet selbst, wann es nachzieht.
- Was hierher gehört: alles, was in einem zweiten Projekt genauso gelten würde. Was nicht
  hierher gehört: Ports, Pfade, Datenstrukturen, Posen, offene Punkte, Chatverlauf, ADRs über
  Projektcode.
- Kennzeichnung verifiziert/Annahme im `knowledge/`-Teil und Go des Users für `rules/` und
  `adr/`: `rules/codex.md`, Abschnitte "Konsistenz" und "Workflow / Pflege".

## Beteiligte Projekte

| Projekt | Ort | Stand der Einbindung |
|---|---|---|
| mec_demo (Wandelbots Nova, Isaac Sim, FastAPI, React) | `C:\Temp\python\mec_demo01\mec_demo`, Remote `code.wabo.run:customer-success/mec_demo` | eingebunden 2026-09-11 |
| tests (IPC Monitor, FastAPI, React) | `c:\my_project\tests`, Remote `code.wabo.run:customer-success/ipc-monitoring` (Branch `master`) | eingebunden 2026-09-11 |
| testautomatisierung (Testprozess-Plan, Python/pytest) | `c:\my_project\testautomatisierung`, Remote `github.com/gdwbdd/Testautomatisierung` (Branch `main`) | eingebunden 2026-09-11 (Git-Init am selben Tag) |
| ALPLA (P&P, Einzelfunktionen über SPS — Annahme aus README) | `c:\my_project\ALPLA`, Remote `github.com/gdwbdd/ALPLA` (Branch `main`) | eingebunden 2026-09-21 aus den Vorlagen |
