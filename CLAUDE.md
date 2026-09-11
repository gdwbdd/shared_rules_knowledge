# CLAUDE.md (shared_rules_knowledge)

Dieses Repo ist selbst die Single Source of Truth für projektübergreifende Regeln und Wissen.
Wird es direkt geöffnet, gelten dieselben Regeln wie überall:

@rules/codex.md
@rules/lernprozesse.md

## Zuerst lesen
1. `README.md` — Zweck, Struktur, Einbinden, Pflege
2. `rules/workbasis_struktur.md` — wie Projekte dieses Repo nutzen
3. `adr/README.md` — Entscheidungen dieses Repos
4. `knowledge/INDEX.md` — Domänenwissen

## Regeln für Änderungen an diesem Repo
- Änderungen an `rules/` und neue ADRs nur nach ausdrücklichem Go des Users.
- Neue Fakten in `knowledge/` mit Kennzeichnung verifiziert (wie/wann) oder Annahme.
- Commit und Push zusammen, nur auf Wort des Users.
- Kein Projektspezifisches (Ports, Pfade, Datenstrukturen, offene Punkte) hier ablegen.
