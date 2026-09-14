# Claude Code und VS Code — Umgebung, Mechanik, Fallstricke

> Ersetzt `~/.claude/frame_settings.md` (2026-08-11 bis 2026-09-11) als Pflegeort für
> projektübergreifende Rahmenbedingungen von Georg Dude (georg.dude@wandelbots.com).
> Methode je Eintrag in Klammern.

## Account und Modelle
- Claude Code läuft über den **Claude-Team-Plan der Organisation Wandelbots** (OAuth-Login,
  `organizationType: claude_team`), nicht über eigenen API-Key. Rolle `organizationRole: "user"`,
  kein Admin/Owner: Modell-Freigaben kann der User nicht selbst ändern (verifiziert 2026-08-11
  in `~/.claude.json`). Wunsch nach mehr Modellen → Admin/Owner der Wandelbots-Organisation in
  der Anthropic-Console; Status: zurückgestellt ("erstmal so weiterarbeiten").
- Bevorzugte Sprache: Deutsch.

## Instruktionsdateien und Importe (verifiziert Claude-Code-Doku "memory", 2026-09-11)
- Ladeordnung: `~/.claude/CLAUDE.md` (gilt für alle Projekte) → Projekt-`CLAUDE.md` →
  `CLAUDE.local.md`; alle werden **konkateniert**, nichts überschreibt.
- `@pfad` in einer CLAUDE.md importiert eine Datei; relative Pfade gelten relativ zur
  importierenden Datei, absolute und `~`-Pfade gehen ebenfalls; maximal 4 Ebenen tief;
  importierte Dateien dürfen selbst importieren. Nicht innerhalb von Code-Spans/-Blöcken.
- `.claude/rules/*.md` (Projekt) und `~/.claude/rules/` (User) sind dokumentiert; per
  Frontmatter `paths:` lassen sich Regeln auf Dateimuster begrenzen.
- Submodul-Inhalte beim `@`-Import: nicht dokumentiert, aber **verifiziert 2026-09-11** in
  mec_demo, tests und Testautomatisierung: frische `claude -p`-Instanz (Extension-Binary
  `resources/native-binary/claude.exe` 2.1.266, `--max-turns 1`, ohne Werkzeuge) zitierte
  Inhalte aus `docs/shared_rules_knowledge/rules/*.md` und listete beide Importe als geladen.
  Dateien, die in `CLAUDE.md` nur unter "Zuerst lesen" stehen, sind NICHT im Kontext.
- Nicht-interaktiver Test einer Einbindung: im Projektordner
  `claude.exe -p "<Frage nach einem Wortlaut aus der importierten Datei>" --max-turns 1
  --output-format text`. Die CLI ist nicht im PATH; das Binary liegt in der VS-Code-Extension.
- Ein noch nicht "trusted" Workspace ignoriert `permissions.allow` aus `.claude/settings.json`
  (Hinweis der CLI); einmal interaktiv öffnen und den Trust-Dialog bestätigen.
- `/memory` bzw. `/context` zeigen, welche Instruktionsdateien geladen sind.

## Auto-Memory (`~/.claude/projects/<slug>/memory/`)
- Beim Start werden nur die ersten 200 Zeilen / 25 KB von `MEMORY.md` geladen; andere
  Dateien liest der Assistent nur auf Nachfrage (Doku, verifiziert 2026-09-11).
- Der Slug wird aus dem Git-Repo abgeleitet; alle Worktrees eines Repos teilen ein Memory.
  Rechner-/Profilwechsel oder ein anderes Repo → anderes, leeres Memory. Teilen über Repos
  hinweg nur per `CLAUDE_CODE_PROJECT_DIR_NAME` (nicht genutzt).
- **Verlustfälle:** 2026-07-29 (mec_demo, Rechnerwechsel), 2026-08-12 (tests, neue Hardware),
  2026-08-10→09-11 (10 Dateien ohne Indexeintrag, unsichtbar). Folge: Memory ist nur Zeiger,
  Regeln und Wissen leben in `shared_rules_knowledge` und in den Projekt-Repos (ADR 001).

## Kontextfenster
- Der Auslösepunkt der automatischen Kompaktierung ist als **Token-Zahl** konfigurierbar, nicht
  als Prozent: Setting `autoCompactWindow` (in `.claude/settings.local.json` oder global),
  Befehl `/autocompact`, Umgebungsvariable `CLAUDE_CODE_AUTO_COMPACT_WINDOW` (verifiziert
  Doku 2026-09-11). mec_demo: `autoCompactWindow: 550000` (55 % von 1M).
- Der Assistent kann Kompaktierung nicht selbst auslösen; nur automatisch am Schwellwert oder
  durch den User mit `/compact` (optional mit Hinweis, was erhalten bleiben soll).
- Bei Kompaktierung geht alles verloren, was nicht in Dateien steht → Zwischenstände in
  `open_points.md`/`chat_log.md` festhalten.

## Tastenkürzel (verifiziert 2026-08-11)
- **Nachricht senden:** `Strg+Enter`, **Zeilenumbruch:** `Enter`.
- VS-Code-Extension: Setting `claudeCode.useCtrlEnterToSend: true` in
  `%APPDATA%\Code\User\settings.json` (aus `package.json` der Extension, Default `false`).
- `~/.claude/keybindings.json` gilt **nur für die CLI**, die Extension ignoriert sie stillschweigend.
- Weitere globale Settings: `claudeCode.preferredLocation: "panel"`,
  `chat.viewSessions.orientation: stacked`; Projekt `.vscode/settings.json`:
  `chat.tools.terminal.autoApprove: true`.

## Dateien im Profil (`~/.claude/`)
| Datei | Zweck |
|---|---|
| `settings.json` | globale Berechtigungen, `additionalDirectories`, Modell |
| `keybindings.json` | CLI-Tastenkürzel |
| `policy-limits.json`, `remote-settings.json` | von Anthropic bzw. Organisation vorgegeben |
| `.credentials.json` | Zugangsdaten — nie lesen oder dokumentieren |
| `projects/<slug>/memory/` | Auto-Memory (nur Zeiger) |
| `frame_settings.md` | seit 2026-09-11 nur Verweis auf diese Datei |

## Git und Netz
- GitHub: `gh` nicht installiert; `git` über HTTPS funktioniert mit dem Git Credential Manager
  (auch für private Repos der Wandelbots-Organisation). WebFetch auf private Repos → 404.
- Sparse-Clone: `git clone --depth 1 --filter=blob:none --sparse <url>`; `git sparse-checkout
  add` einzelner Dateien braucht `--skip-checks`.
- Projekt-Remote mec_demo: SSH `git@code.wabo.run:...` (SSH-Agent läuft).
- Backend-Neustart eines Nova-Projekts verliert Laufzeit-Overrides (z. B. IPC-Adresse) → nach
  Neustart erneut setzen (Projektdetails im Projekt).

## Shell-Fallstricke (Windows)
- **Git Bash** schreibt absolute POSIX-Pfade in Argumenten um (`/World/...` →
  `C:/Program Files/Git/World/...`). Prim-Pfade und ähnliche Argumente aus Python senden.
- `.gitignore` global unter `~/.config/git/ignore` kann Dateien wie
  `.claude/settings.local.json` ausblenden; `git check-ignore -v` zeigt die Quelle.
- Prozesse aus dem eigenen VS-Code-Terminal des Users sind für die Werkzeug-Shell des
  Assistenten unsichtbar und unkillbar, Ports aber geteilt (Lernprozess 2026-08-14).
- Kompilierte `.pyc` unter `__pycache__` sind in mec_demo historisch getrackt; seit 2026-09
  werden sie nicht mehr mitcommittet (Staging mit `':!**/__pycache__/**'`).
