# CLAUDE.md

Diese Datei wird von Claude Code automatisch bei Sessionstart geladen. Sie enthält keinen
eigenen Regeltext, nur Importe und Leseordnung.

## Geteilte Regeln (Single Source of Truth, Submodul `docs/shared_rules_knowledge/`)
@docs/shared_rules_knowledge/rules/codex.md
@docs/shared_rules_knowledge/rules/lernprozesse.md

Ist das Submodul leer: `git submodule update --init`. Repo: https://github.com/gdwbdd/shared_rules_knowledge

## Zuerst lesen (in dieser Reihenfolge)
1. `docs/workbasis/codex.md` — projektspezifische Regeln und Abweichungen
2. `docs/workbasis/open_points.md` — offene Punkte/Wiedervorlage. Session-Start: ZUERST lesen. Session-Ende: ZULETZT aktualisieren.
3. `docs/workbasis/chat_log.md` — Chatverlauf/Kontext (selbstständig pflegen)
4. `docs/workbasis/knowledge/INDEX.md` — Projektwissen; `knowledge/terminologie.md` — Projektglossar
5. `docs/shared_rules_knowledge/knowledge/terminologie.md` — Domänenglossar + Invarianten
6. `docs/workbasis/adr/README.md` — Projektentscheidungen; Prozess in `docs/shared_rules_knowledge/adr/README.md`

## Geltungsbereich
Dieselben Regeln gelten für GitHub Copilot (`.github/copilot-instructions.md`). Repo-Dateien
sind autoritativ; der Memory-Store des Assistenten ist nur Zeiger.

## Handoff-Pflicht
Jede "speichern & später vorlegen"-Zusage sofort in `docs/workbasis/open_points.md`. Neue
mögliche Regel erkannt → prüfen, ob im geteilten oder Projekt-Codex vorhanden, sonst nachfragen.
