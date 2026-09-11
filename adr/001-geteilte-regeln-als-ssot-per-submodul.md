# ADR 001: Geteilte Regeln und Wissen leben in einem eigenen Repo und werden per Git-Submodul eingebunden

**Status**: Angenommen
**Datum**: 2026-09-11
**Beteiligte**: Georg Dude (Entscheidung), Claude Code (Analyse, Vorschlag)

## Entscheidung

Alles, was in mehr als einem Projekt gilt — Regeln der Zusammenarbeit, gelernte Lektionen,
ADR-Prozess, Domänenwissen zu Nova und Isaac Sim, Fakten zur Werkzeugumgebung — wird aus den
Projekten herausgezogen und in `github.com/gdwbdd/shared_rules_knowledge` gepflegt. Projekte
binden dieses Repo als Git-Submodul unter `docs/shared_rules_knowledge/` ein und importieren die
Regeln per `@`-Import in ihrer `CLAUDE.md`. Projekte behalten nur Projektspezifisches.

Y-Satz: Im Kontext mehrerer Projekte mit denselben Arbeitsregeln, angesichts gemessener Drift
zwischen Kopien und wiederholtem Verlust des Assistenten-Memorys, wählen wir ein eigenes Repo
plus Submodul, um genau einen Pflegeort zu haben, der Rechnerwechsel überlebt, und nehmen die
Submodul-Disziplin (Zeiger nachziehen) in Kauf.

## Kräfte

- **Drift, gemessen 2026-09-11:** Der Codex in `c:\my_project\tests` war eine Kopie vom
  2026-08-13. In mec_demo kamen danach hinzu: "Kommunikation statt Aktionismus" (08-20),
  Markierung verifiziert/Annahme (09-09), ADR und Vokabular (09-11), Tagesstart-Regeln.
  Nichts davon erreichte das Schwesterprojekt (verifiziert per Dateivergleich).
- **Memory-Verlust, dreimal:** kompletter Verlust bei Rechnerwechsel (2026-07-29 mec_demo,
  2026-08-12 tests); 10 Regel-Dateien vom 2026-08-10 nie im Index `MEMORY.md`, damit einen
  Monat unsichtbar (verifiziert per Dateidaten und Claude-Code-Doku: nur `MEMORY.md` wird
  beim Start geladen). Der User musste Regeln wiederholt neu beibringen.
- **Zwei Assistenten:** GitHub Copilot liest `.github/copilot-instructions.md` im Repo; ein
  Mechanismus außerhalb des Repos erreicht ihn nicht.
- **User-Vorgabe:** "SSoT! Mehrere gepflegte Stellen bringen Fehler."

## Betrachtete Alternativen

- **Kopieren je Projekt (bisheriger Zustand)** — genau die gemessene Drift; jede Regeländerung
  müsste n-fach nachgezogen werden.
- **Assistenten-Memory als Träger** — rechner- und pfadgebunden (Slug aus dem Git-Repo), nur
  der Index wird geladen, bereits dreimal verloren; für Copilot unsichtbar.
- **Nur globale `~/.claude/CLAUDE.md` ohne Repo** — gilt zwar in allen Projekten (Doku
  verifiziert), liegt aber außerhalb jeder Versionierung und geht beim Rechnerwechsel verloren;
  Copilot sieht sie nicht.
- **Ein lokaler Clone plus globale `~/.claude/CLAUDE.md` mit absolutem `@`-Import (Option B)** —
  null Einrichtung pro Projekt und immer aktuell, aber rechnergebunden (der Verlustpfad vom
  2026-08-12), Copilot sieht nichts, Projekte könnten keinen Stand pinnen.
- **Git-Submodul plus relativer `@`-Import (Option A, gewählt)** — geht mit dem Repo mit,
  überlebt Rechnerwechsel zusammen mit dem Projekt, Copilot erreicht es über den Repo-Pfad,
  jeder Stand ist pro Projekt festgepinnt. `@`-Importe mit relativen Pfaden bis vier Ebenen tief
  sind dokumentiert (verifiziert 2026-09-11, Claude-Code-Doku "memory"). Dass Submodul-Inhalte
  beim Import wie normale Dateien behandelt werden, ist nicht dokumentiert: Annahme, nach der
  Einrichtung per `/context` prüfbar.

## Konsequenzen

- Ein Pflegeort. Regeländerungen werden hier committet und gepusht; Projekte ziehen den Zeiger
  bewusst nach (`git submodule update --remote`), auf Wort des Users.
- Klonen eines Projekts braucht `--recurse-submodules` oder `git submodule update --init`;
  sonst ist `docs/shared_rules_knowledge/` leer und die Importe laufen ins Leere. Das ist die
  akzeptierte Kosten-Seite.
- Projekt-Codizes schrumpfen auf Projektspezifisches; `CLAUDE.md` und
  `copilot-instructions.md` enthalten nur Leseordnung und Importe.
- Projekt-ADRs (in mec_demo 001–006) bleiben im Projekt: sie entscheiden über Projektcode.
  Der ADR-Prozess (README, TEMPLATE) lebt hier.
- Assistenten-Memory wird auf Zeiger reduziert; die 10 unindexierten Dateien werden nach
  Übernahme gelöscht.
- Optional später: eine globale `~/.claude/CLAUDE.md`, die nur auf einen lokalen Clone zeigt,
  für Projekte ohne Submodul. Das wäre kein zweiter Pflegeort, nur ein zweiter Zugang.
  Bewusst verschoben.
- Bewusst verschoben: Einbindung in `c:\my_project\tests` (eigene Session).

## Quellen

- mec_demo `docs/workbasis/chat_log.md`, Eintrag 2026-09-11 (Analyse, Freigabe "A", Punkte 1–6)
- mec_demo `docs/workbasis/knowledge/novaflow_referenz.md` (ADR-Vorbild)
- Claude-Code-Doku, Seite "memory": `@`-Importe, Ladeordnung `~/.claude/CLAUDE.md` → Projekt,
  Auto-Memory lädt nur `MEMORY.md`, Slug aus dem Git-Repo
- `knowledge/claude_code_umgebung.md` (Memory-Fakten, Verlustfälle)
