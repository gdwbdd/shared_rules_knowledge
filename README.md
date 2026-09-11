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
5. Beim Klonen eines Projekts: `git clone --recurse-submodules ...` oder danach
   `git submodule update --init`.

## Pflege

- **Ändern nur hier.** Regeländerungen, neue Lektionen, neue Domänenfakten werden in diesem
  Repo committet und gepusht (commit und push zusammen).
- Projekte holen den neuen Stand mit `git submodule update --remote docs/shared_rules_knowledge`
  und committen den neuen Submodul-Zeiger. Der Zeiger pinnt bewusst einen Stand; ein Projekt
  entscheidet selbst, wann es nachzieht.
- Was hierher gehört: alles, was in einem zweiten Projekt genauso gelten würde. Was nicht
  hierher gehört: Ports, Pfade, Datenstrukturen, Posen, offene Punkte, Chatverlauf, ADRs über
  Projektcode.
- Jede Aussage im `knowledge/`-Teil ist als **verifiziert (wie/wann)** oder **Annahme**
  gekennzeichnet (Regel aus `rules/codex.md`, Abschnitt Konsistenz).
- Änderungen an `rules/` und am ADR-Verzeichnis brauchen das ausdrückliche Go des Users
  (Regel "Neue mögliche Regel erkannt → nachfragen").

## Beteiligte Projekte

| Projekt | Ort | Stand der Einbindung |
|---|---|---|
| mec_demo (Wandelbots Nova, Isaac Sim, FastAPI, React) | `C:\Temp\python\mec_demo01\mec_demo`, Remote `code.wabo.run:customer-success/mec_demo` | eingebunden 2026-09-11 |
| tests (IPC Monitor) | `c:\my_project\tests` | offen, eigene Session |
