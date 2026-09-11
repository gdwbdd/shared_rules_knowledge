# Wandelbots Nova — SDK und API, verifizierte Fakten

> Herkunft: mec_demo `knowledge/omniverse_collision_recherche.md` (2026-08-19 bis 2026-09-09)
> und `novaflow_referenz.md`. Methode je Eintrag in Klammern. Projektzahlen (IPs, Cell-IDs,
> Roboterlisten) bleiben im Projekt.

## Quellen
- Nova-API Swagger-UI je Cluster: `http://<nova-host>/api/v2/ui/`; `openapi.json` ist
  cell-spezifisch geroutet (Pfad per Raten nicht gefunden, 2026-08-19).
- Doku: `https://docs.wandelbots.io` (Kollisions-Guide 26.5: `api-guide-collision`).
- SDK-Repo: `github.com/wandelbotsgmbh/wandelbots-nova`; DeepWiki
  `deepwiki.com/wandelbotsgmbh/wandelbots-nova` (Abschnitt 5.2 Collision-Free Motion Planning).
- Installiertes Paket: `.venv/Lib/site-packages/wandelbots_api_client/` (Quelle für Modelle
  und API-Klassen, verlässlicher als Raten von REST-Pfaden).

## Kollisions-API (SDK, verifiziert im Paketquellcode 2026-08-19 / 2026-09-09)
- Geometrie-Primitive: `Sphere`, `Box`, `Cylinder`, `Capsule`, `ConvexHull`, `Rectangle`,
  `RectangularCapsule`, je in `api.models.Collider {shape, pose: Optional[Pose2], margin: float=0}`
  (margin in mm, vergrößert die Form in alle Richtungen).
- `CollisionSetup`-Felder: `colliders` (ColliderDictionary), `link_chain` (LinkChain), `tool`
  (`Tool` = RootModel Dict[str, Collider], "attached to the flange frame"),
  `self_collision_detection` (Default True).
- Store-APIs am Gateway (`cell._api_client.<api>`, Muster wie `controller_api`):
  - `store_collision_components_api`: `list_stored_colliders(cell)`, `get_stored_collider`,
    `store_collider(cell, name, collider)` (create-or-update), `delete_stored_collider`;
    außerdem `store_collision_tool`, `store_collision_link_chain`.
  - `store_collision_setups_api`: `list_stored_collision_setups[_keys]`,
    `get_stored_collision_setup(cell, setup)`, `store_collision_setup(cell, setup, collision_setup)`,
    `delete_stored_collision_setup`.
- Offizieller Workflow (Guide 26.5): Link-Collider holen (`GET .../motion-groups/{mg}/collision-model`
  oder `.../description`) → Link-Chain speichern → Tool speichern → Umgebungs-Collider speichern
  → zu `CollisionSetup` kombinieren → speichern → `POST .../trajectory-planning/plan-collision-free`
  (Setup inline oder per Referenz).
- LinkChain: Referenzframe eines Links = nach Anwendung aller DH-Sätze bis einschließlich
  Link-Index; Frame des letzten Links = Flansch. Benachbarte Links werden nicht gegeneinander
  geprüft. Beispiel KUKA KR6 R900_2: 6 Links (0..5), Link 5 = 9 konvexe Hüllen, Margin 1.0 mm.
- Normale Motion-Actions nehmen `collision_setup` nur zur **Validierung** (Fehler bei Kollision,
  kein Umbau); die Action `collision_free` sucht aktiv einen anderen Weg (Algorithmen
  `RRTConnect` probabilistisch vollständig, `MidpointInsertion` deterministisch).
- **Einschränkung (Guide 26.5):** Kollisionsplanung über mehrere Motion-Groups ist nur
  experimentell; synchrone Ausführung über mehrere Motion-Groups ist nicht möglich, Workaround
  sequentielle Aufrufe. Betrifft jede parallele Roboteransteuerung per `asyncio.gather`.
- Safety-Zonen: Self-Collision auf `false` ("die Safety-Modelle sind dafür nicht geeignet").
- Blending: ist kein Blending möglich, kommt trotzdem eine gestitchte Trajektorie zurück.
- Nova ↔ Isaac: keine automatische Synchronisierung zwischen Nova-Store-Collidern und
  Isaac-Szene; zwei getrennte Systeme (verifiziert 2026-08-20).

## Planen ohne Bewegung (verifiziert live 2026-09-09)
- `MotionGroup.plan(actions, tcp, start_joint_position=None, ...)`: mit `start_joint_position`
  lässt sich eine Planung aus einer BELIEBIGEN Pose prüfen, ohne den Roboter zu bewegen. Basis
  aller Verifikationen ohne Hardware-Risiko.
- **Startkollision:** HTTP 422, `PlanValidationError(loc=['start_joint_position'],
  msg="Joint position [...] results in a collision ...")`; der Rohtext enthält den kompletten
  Request-Body (hunderte KB) — für Anzeige die `msg` extrahieren.
- **Wegkollision:** `error_feedback.collisions` mit den beteiligten Shapes.
- Der SDK-Rückfall `collision_free()` wirft bei Misserfolg nur
  `ValueError("No collision free trajectory found")`; Novas Text geht verloren. Wer den
  Verursacher wissen will, muss die ursprüngliche Exception auswerten oder per Einzel-Isolation
  der Collider neu planen (mec_demo ADR 004).
- Ein linearer Plan kann in eine Handgelenk-Singularität laufen und dadurch fehlschlagen, ohne
  dass eine Kollision vorliegt; für Tests `cartesian_ptp` verwenden.

## Kontaktverhalten des Checkers (gemessen 2026-09-09, KUKA, Schnellwechsler-Platte vs. Link 5)
- Kollision nur im Band −0.6..0.0 mm um den Flächenkontakt; 0.7–4 mm Durchdringung gilt als
  frei; 6 mm wieder Kollision; deterministisch; kippt mit Gleitkomma-Details der Pose.
- Mit anderer Geometrie (Box, CobotPump-Hülle) in Flanschkontakt NICHT reproduzierbar →
  geometriespezifisch, nicht verallgemeinern.
- `tool`-Feld: eine Umgebungs-Kugel INNERHALB der Werkzeuggeometrie wird nur mit gesetztem
  `tool` abgelehnt → Werkzeug wird gegen Umgebung geprüft. Ob ein `tool` in Flanschkontakt mit
  dem letzten Link toleriert wird: UNVERIFIZIERT.

## SDK-Importfalle (Python, verifiziert 2026-09-09)
- `nova/api.py` setzt `__path__` auf `wandelbots_api_client/v2_pydantic`. Ein Submodul-Import
  (`from nova.api.exceptions import X`, `import nova.api.models`) führt die SDK-Datei ein
  ZWEITES Mal aus → eigene Klassenobjekte; `except`/`isinstance` gegen echte SDK-Objekte greifen
  nicht (z. B. 404 nicht erkannt).
- Attribut-Import (`from nova.api import exceptions`, `from nova.api import models`) liefert die
  echten Objekte. **Regel: IMMER Attribut-Stil** (Invariante 4).
- Zwischenlösung, falls Klassenidentität unsicher: Statusattribut prüfen
  (`getattr(exc, "status", None) == 404`).

## Session und Zustand (gemessen in novaflow 2026-09-10, siehe `novaflow_referenz.md`)
- **Motion-Group-Claim:** Ein Client mit offener Session auf einer Motion-Group hält sie bis zum
  Schließen — ab Session-Erstellung, nicht ab erster Bewegung. Offenes Jog-Panel/RobotPad
  blockiert jede Routine. Zweite Session → 409; Trajektorie gegen gehaltene Motion-Group →
  "Movement request rejected. Another client is currently executing a 'Jogging' motion!".
  Davon zu unterscheiden: MODE_CONTROL/MODE_MONITOR (blockiert keine Trajektorie).
- **Typen:** `wandelbots_api_client.v2_pydantic`-Klassen direkt verwenden (kein
  Spiegel-Typsystem). Generierte Bindings können von der OpenAPI-Spec abweichen (`Vector3d`:
  Spec `[x,y,z]`, v1-Binding Objekt `{x,y,z}`, v2_pydantic `list[float]`).
- **State per NATS:** `nova.v2.cells.cell.controllers.*.state` (mode, safety_state,
  joint_position, tcp_pose, standstill, joint_limit_reached ...) — Alternative zu REST-Polling.
- `nova.types.Pose` akzeptiert Tupel im Konstruktor und unterstützt den `@`-Operator
  (Komposition); Rotationsvektor-Konvention, identisch mit der Isaac-Extension.

## Logging
- `nova.core.gateway` loggt jeden Request auf INFO; bei UI-Polling flutet das jeden Ring-Log.
  Abhilfe: `logging.getLogger("nova.core.gateway").setLevel(logging.WARNING)` (verifiziert 2026-09-09).
