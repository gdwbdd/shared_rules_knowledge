# Architecture Decision Records — Prozess und Verzeichnis (geteilt)

> Eingeführt 2026-09-11 nach dem Vorbild von wandelbotsgmbh/novaflow `docs/adr`
> (siehe `../knowledge/novaflow_referenz.md`). Ziel des Users: Kontinuität und weniger
> Missverständnisse — den Arbeitsrahmen genauer definieren, nicht eingrenzen.

Ein ADR hält **eine Entscheidung und warum sie gegen die Alternativen gewonnen hat** fest —
nicht, wie das System funktioniert (das steht im Code und im Projektplan). Die Entscheidung
steht zuerst; das Herzstück sind die verworfenen Optionen mit ihrem Grund. Knapp halten.

## Regeln
Die Regeln (ADR je Entscheidung mit Alternativen, `Vorgeschlagen` vor Umsetzung, `Angenommen`
mit Go des Users und dann unveränderlich, Kennzeichnung verifiziert/Annahme, Ort Projekt vs.
geteilt) stehen in `rules/codex.md`, Abschnitt "Entscheidungen festhalten (ADR)". Ergänzend nur
hier:
- Beim Ersetzen bekommt das alte ADR nur den Status `Ersetzt durch ADR-NNN`. Zulässig ist ein
  datierter Nachtrag "Umsetzung"/"Verifikation", der die Entscheidung nicht ändert.
- `open_points.md` des Projekts verweist bei Entscheidungen nur auf das ADR (Status und
  Wiedervorlage bleiben dort); `PROJECT_PLAN.md` beschreibt die Umsetzung.

## Neues ADR anlegen
1. `TEMPLATE.md` nach `NNN-kurzer-titel.md` kopieren (nächste freie Nummer im jeweiligen Verzeichnis).
2. Entscheidung → Kräfte → Alternativen → Konsequenzen → Quellen ausfüllen.
3. Als `Vorgeschlagen` öffnen, nach Freigabe auf `Angenommen`; Verzeichnistabelle ergänzen.

## Verzeichnis (dieses Repo)
| Nr. | Titel | Status |
|---|---|---|
| [001](001-geteilte-regeln-als-ssot-per-submodul.md) | Geteilte Regeln und Wissen als SSoT in eigenem Repo, Einbindung per Git-Submodul | Angenommen |
