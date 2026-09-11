# Terminologie — kanonisches Domänen-Glossar (geteilt)

> Eingeführt 2026-09-11 nach dem Vorbild von novaflow `docs/terminology.md`. **Single Source of
> Truth für Vokabular über Projekte hinweg:** Widerspricht ein Gebrauch (Chat, Code, UI, Doku)
> diesem Glossar, ist der Gebrauch falsch — oder das Glossar ist veraltet und wird bewusst
> korrigiert, nicht umgangen. Jeder Eintrag nennt die autoritative Quelle.
> Projektbegriffe (Bewegungsnamen, Posen-Felder, UI-Texte) stehen im Projektglossar
> (`docs/workbasis/knowledge/terminologie.md` des Projekts).

## Collider — vier Bedeutungen

- **Umgebungs-Collider** — statische Hindernisse in `CollisionSetup.colliders`.
- **Werkzeug-Collider (`tool`)** — flanschfeste Geometrie in `CollisionSetup.tool`
  (`Tool` = Dict[str, Collider], "attached to the flange frame"); wird gegen Umgebung
  geprüft, ist nie selbst Umgebung. *Quelle:* Nova-Doku `Tool`, `nova_sdk.md`.
- **Link-Kette (`link_chain`)** — Robotergliedmaßen aus Nova
  (`get_motion_group_collision_model`); Roboter-`visuals` in Isaac sind deshalb als Collider
  deaktiviert. Benachbarte Links werden nicht gegeneinander geprüft.
- **Nova-manueller Collider** — im Nova-Store zellweit gespeichert
  (`store_collision_components_api`), unabhängig von Isaac.
- **Cell-Prim** — Scope eines Roboters in einer Isaac-Szene, strukturell aus dem
  Roboter-Prim-Pfad abgeleitet (Hierarchie, keine Namens-Regex). Alles unterhalb gilt für
  diesen Roboter, alles außerhalb aller Cell-Prims ist **global** (gilt für alle).
  *Quelle:* mec_demo ADR 002. Die Ableitungsregel (welche Segmente) ist projektspezifisch.

## Setup — Zustände

- **gespeichertes Setup** — `CollisionSetup` im Nova-Store (`store_collision_setups_api`),
  ID frei wählbar (Konvention mec_demo: Controller-Name).
- **Live-Sweep** — aktueller Isaac-Zustand über `POST /physics/collision/sweep`; liefert nur
  aktivierte, exportierbare Collider.
- **Hybrid-Setup** — zur Laufzeit zusammengesetzt aus gespeichertem Setup, live gelesenen
  globalen Collidern und manuellen Collidern, mit Rückfall auf den Live-Sweep der eigenen Zelle.
  *Quelle:* mec_demo ADR 001.
- **veraltet** — gespeichertes Setup weicht vom Live-Stand ab (Warnung, kein Abbruch).

## Abstände — Margins und Luft

- **Collider-Margin** — Sicherheitsabstand je Collider (Nova-Feld `margin`, mm), vergrößert die
  Form in alle Richtungen.
- **Prüf-Margin** — temporär vergrößerte Umgebungs-Collider nur für einen Nähe-Test; bewegt nichts.
- **Ziel-Luft** — gewünschter Mindestabstand nach einem Rückzug; bestimmt die Distanz.
- **Luft** — gemessener Mindestabstand Werkzeug/Werkstück zu Umgebung. Vertex-Punktabstände
  übersehen Flächendurchdringung dünner Körper; Halbraum-Test (ConvexHull-Ebenen) nötig.

## Kollisionsarten (Novas Antworten)

- **Startkollision** — HTTP 422, `PlanValidationError(loc=['start_joint_position'],
  msg="... results in a collision")`: schon die Startpose kollidiert, nichts planbar.
- **Wegkollision** — `error_feedback.collisions`: der direkte Weg kollidiert; Umplanung per
  `collision_free` möglich.
- **Umplanung (`collision_free`)** — Novas RRT-Neuplanung; ein anderer Weg, kein Rückzug.
- **Kontaktband** — gemessenes Verhalten des Nova-Checks bei Flächenkontakt (siehe
  `nova_sdk.md`); geometriespezifisch, nicht verallgemeinern.

## Frames

- **Flansch** — TCP `Flange` (Index 0), Referenzframe des `tool` und des letzten Links.
- **TCP** — konfigurierter Arbeits-TCP (Werkzeugspitze), per `tcp_index`/Name gewählt.

## Betriebszustände (Nova, gemessen in novaflow)

- **Halt** (Verb) — absichtliches Anhalten durch Bediener oder Ablauf; UI-Label "Stop".
- **motion-arrest** — der Mechanismus, dass der Roboter steht (`PauseMovementRequest`,
  decel-to-standstill); kein Terminus für Ergebnisklassen.
- **HALTED ≠ FAILURE**: HALTED ist absichtlich, FAILURE heißt "etwas ist gebrochen" (nie
  Auto-Resume). **Pause ≠ Halt.** *Quelle:* `novaflow_referenz.md`.
- **Motion-Group-Claim** — eine offene Session (Jog, RobotPad) hält die Motion-Group exklusiv;
  zweite Session → 409. *Quelle:* `nova_sdk.md`.

## Arbeitsrahmen

- **ADR** — Architecture Decision Record: eine Entscheidung mit verworfenen Alternativen;
  `Vorgeschlagen` → `Angenommen` (unveränderlich) → `Ersetzt durch`.
- **verifiziert (wie/wann) / Annahme** — Pflichtmarkierung jeder Begründung.
- **SSoT** — Single Source of Truth: genau ein Pflegeort je Inhalt.
- **Workbasis** — `docs/workbasis/` eines Projekts (Codex, open_points, chat_log, knowledge, adr).

## Invarianten (nicht verletzen)

1. **Omniverse nicht erreichbar ist kein Fehler.** Fehlende Collider → Warnung, die Aktion läuft.
2. **Werkzeug ist nie Umgebung.** Flanschfeste Geometrie gehört in `tool`.
3. **Keine Namens-Regex auf Prim-Pfaden.** Zugehörigkeit kommt aus der Hierarchie.
4. **Nova-SDK-Importe nur im Attribut-Stil** (`from nova.api import models|exceptions`).
5. **Jede Begründung ist "verifiziert (wie/wann)" oder "Annahme".**
6. **Angenommene ADRs werden nicht editiert, sondern ersetzt.**
7. **User-Code ist die Referenz**, Änderungen additiv.
8. **Ein Inhalt, ein Pflegeort** (SSoT); Kopien werden durch Verweise ersetzt.

Projektinvarianten (z. B. "Kontakt nur bei Pick und Place") stehen im Projektglossar.
