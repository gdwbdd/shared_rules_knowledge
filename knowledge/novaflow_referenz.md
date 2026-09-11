# Referenz: wandelbotsgmbh/novaflow — ADR-Praxis, Terminologie, Nova-Fakten

> User-Vorgabe 2026-09-11: dieses Material definiert den Arbeitsrahmen des Assistenten genauer
> ("nicht eingrenzen, aber Kontinuität, verringerte Missverständnisse ermöglichen").

## Zugriff
- Repo ist **privat**: `github.com`/`api.github.com` antworten anonym mit 404, `gh` ist nicht
  installiert. **Git funktioniert** (Git Credential Manager hält die GitHub-Anmeldung des Users):
  flacher Sparse-Clone ins Session-Scratchpad, z. B.
  `git clone --depth 1 --filter=blob:none --sparse https://github.com/wandelbotsgmbh/novaflow.git`
  + `git sparse-checkout set docs/adr` (einzelne Dateien: `--skip-checks`).
- Gelesen (Stand main @ 9d61084, 2026-09-10): `docs/adr/README.md`, `TEMPLATE.md`, ADR 026
  (Musterbeispiel), 028, 040; `docs/terminology.md`; `docs/novaflow-core-concepts.md`.

## Links (im Browser mit GitHub-Login des Users)
- https://github.com/wandelbotsgmbh/novaflow/tree/main/docs/adr — ADR-Verzeichnis (45 Dateien)
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/adr/README.md — ADR-Regeln
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/adr/TEMPLATE.md — ADR-Vorlage
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/adr/026-run-result-verdict-and-failure-log.md — Musterbeispiel
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/adr/028-adopt-nova-client-types-directly.md — Nova v2_pydantic-Typen
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/adr/040-nova-service-boundary.md — Nova NATS-State
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/terminology.md — Glossar + Invarianten
- https://github.com/wandelbotsgmbh/novaflow/blob/main/docs/novaflow-core-concepts.md — Kernkonzepte

## ADR-Praxis (docs/adr)
- Ein ADR hält **eine Entscheidung und warum sie gegen die Alternativen gewonnen hat** fest —
  nicht, wie das System funktioniert (das steht im Code).
- Aufbau: **Decision** (zuerst, "We will ..."; optional Y-Statement) → **Forces** (nur was die
  Frage zur Frage machte) → **Alternatives considered** (Herzstück: jede Option mit Einzeiler,
  warum sie verlor; die gewählte zuletzt; validierende Spikes nennen) → **Consequences**
  (Gewinne UND bewusst akzeptierte Kosten, bewusst Verschobenes) → **References**.
- Kopf: Status (Proposed | Accepted | Superseded by ADR-NNN), Date, Authors, Supersedes.
- **Accepted = unveränderlich.** Änderungen kommen als neues ADR; in der Praxis werden zusätzlich
  datierte "Update"-Abschnitte VOR die alte Entscheidung gesetzt und der Status-Satz erweitert
  (z. B. ADR 028: "Accepted — re-affirmed by ADR-029").
- Verifikation dort ausdrücklich als "measured <Datum>", "validated live", "not gated, by
  construction" gekennzeichnet — deckt sich mit unserer Regel "verifiziert (wie) / Annahme".

## Terminologie-Glossar (docs/terminology.md)
- **Single Source of Truth für Vokabular**: "If usage elsewhere conflicts with this doc, the
  usage is wrong (or this doc is stale and should be fixed deliberately, not worked around)."
- Jeder Eintrag nennt die **autoritative Quelle** (ADR/Spec/Code).
- Explizite **Invarianten** ("do not violate"), z. B. HALTED ≠ FAILURE, Halt (Verb) ≠
  motion-arrest (Mechanismus), Pause ≠ Halt.
- Produktsprache vs. Code: UI sagt "Flow"/"Stop", Code sagt "tree"/"Halt" — bewusst getrennt,
  keine API-Umbenennung um der UI willen.

## Nova-Plattform-Fakten, dort gemessen
- **Motion-group claim:** siehe `nova_sdk.md`, Abschnitt Session und Zustand (gemessen
  2026-09-10 in novaflow). Kandidat-Erklärung für den mec_demo-Befund 2026-09-01
  ("cell3kuka: already used for 'ExecuteTrajectory'", RobotPad offen); dort nicht nachgemessen.
- **Halt/Stop/Pause/motion-arrest:** siehe `terminologie.md`, Abschnitt Betriebszustände.
- **Typen (ADR 028/029):** `wandelbots_api_client.v2_pydantic` direkt importieren ("drift
  impossible by construction", Version-Pin = SSoT); Abweichungen generierter Bindings von der
  Spec möglich (`Vector3d`).
- **Nova-State per NATS (ADR 040):** `nova.v2.cells.cell.controllers.*.state`.
- Core-Concepts: Trees/Flows aus Sequence/Selector/Parallel/Decision/RepeatUntil/ForEach +
  Action-Nodes; `MotionRoutine` = Segmente aus Nova-Motion-Commands + wait/set_io; Typen
  strukturell identifiziert (ADR 029). Hintergrund, nicht direkt genutzt.
