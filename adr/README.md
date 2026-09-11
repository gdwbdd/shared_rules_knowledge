# Architecture Decision Records — Prozess und Verzeichnis (geteilt)

> Eingeführt 2026-09-11 nach dem Vorbild von wandelbotsgmbh/novaflow `docs/adr`
> (siehe `../knowledge/novaflow_referenz.md`). Ziel des Users: Kontinuität und weniger
> Missverständnisse — den Arbeitsrahmen genauer definieren, nicht eingrenzen.

Ein ADR hält **eine Entscheidung und warum sie gegen die Alternativen gewonnen hat** fest —
nicht, wie das System funktioniert (das steht im Code und im Projektplan). Die Entscheidung
steht zuerst; das Herzstück sind die verworfenen Optionen mit ihrem Grund. Knapp halten.

## Regeln (gelten für dieses Repo und für jedes Projekt-`adr/`)
- Jede Entscheidung, bei der mehrere Wege zur Wahl standen, bekommt ein ADR — bevor sie
  umgesetzt wird: als `Status: Vorgeschlagen` anlegen, mit dem Go des Users auf `Angenommen`.
  Das trennt Plan von Umsetzung.
- **Angenommen = unveränderlich.** Ändert sich die Entscheidung, entsteht ein neues ADR, das das
  alte ersetzt (`Ersetzt: ADR-NNN`); im alten wird nur der Status auf `Ersetzt durch ADR-NNN`
  gesetzt. Zulässig ist ein datierter Nachtrag "Umsetzung"/"Verifikation", der die Entscheidung
  nicht ändert.
- Jede Begründung ist als **verifiziert (wie/wann)** oder **Annahme** markiert.
- `open_points.md` des Projekts verweist bei Entscheidungen nur noch auf das ADR (Status und
  Wiedervorlage bleiben dort); `PROJECT_PLAN.md` beschreibt die Umsetzung.
- Vokabular nach `knowledge/terminologie.md` (geteilt) und dem Projektglossar.
- Ort: Entscheidungen über Projektcode im Projekt (`docs/workbasis/adr/`), Entscheidungen über
  den geteilten Rahmen hier.

## Neues ADR anlegen
1. `TEMPLATE.md` nach `NNN-kurzer-titel.md` kopieren (nächste freie Nummer im jeweiligen Verzeichnis).
2. Entscheidung → Kräfte → Alternativen → Konsequenzen → Quellen ausfüllen.
3. Als `Vorgeschlagen` öffnen, nach Freigabe auf `Angenommen`; Verzeichnistabelle ergänzen.

## Verzeichnis (dieses Repo)
| Nr. | Titel | Status |
|---|---|---|
| [001](001-geteilte-regeln-als-ssot-per-submodul.md) | Geteilte Regeln und Wissen als SSoT in eigenem Repo, Einbindung per Git-Submodul | Angenommen |
