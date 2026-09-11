# Isaac Sim mit Wandelbots-Extension — OmniService-API, verifizierte Fakten

> Herkunft: mec_demo `knowledge/omniverse_collision_recherche.md` (2026-08-19 bis 2026-09-09).
> Extension: `wandelbots.omni` (geprüft 2.50.0 und 2.56.0-dev), Repo
> `github.com/wandelbotsgmbh/wandelbots-isaacsim-extension`; Python-Client
> `wandelbots_isaacsim_api` (im Hersteller-Beispiel genutzt, `requests` gegen dieselben
> Endpunkte funktioniert ebenso).

## Grundlagen
- Basis-URL: `http://<host>:8011/omniservice/api/v2`, Swagger unter `/ui`, Spezifikation
  `GET /openapi.json` (47 Endpunkte, 2026-08-19). `GET /version` liefert die Extension-Version.
- Das Nova-Python-SDK kennt Isaac/Omniverse nicht (nur Rerun als Viewer); jede Isaac-Anbindung
  ist eine eigene Ergänzung gegen diese REST-API (verifiziert 2026-08-19, DeepWiki).
- **Mehrere Instanzen sind möglich und sehen anders aus:** zwei Isaac-Sim-Instanzen (lokal und
  im Netz) mit verschiedenen Extension-Versionen lieferten verschiedene Collider-Mengen
  (verifiziert per `GET /version`, 2026-08). Immer angeben, gegen welche Instanz gemessen wurde.

## Endpunkte (Stand 2026-08-19)
| Bereich | Endpunkte |
|---|---|
| Auth | `POST /auth/token` |
| Motion Groups | `GET/POST/DELETE /manipulators/motion-groups`, `PUT/GET/DELETE /manipulators/motion-groups/{prim_path}`, `GET /stage/motion-groups` |
| Physik/Kollision | `POST /physics/collision/sweep`, `PATCH /prims/physics/colliders/`, `PATCH /prims/physics/joints` |
| Prims | `GET/PUT /prims/poses`, `GET/PUT/DELETE /prims/poses/default`, `POST /prims/poses/default/reset`, `GET/POST /prims/poses/relative`, `GET/PUT /prims/selected`, `PUT/GET/DELETE /prims/labels`, `PUT/DELETE /prims/metadata`, `PATCH /prims/visibility` |
| Kameras | `GET/PUT /periphery/cameras/active`, `GET /periphery/cameras/prims`, `GET /periphery/cameras/capture/{color,depth,pointcloud,normals,bounding-box-2d,bounding-box-3d,instance-segmentation,semantic-segmentation}` |
| Trajektorien | `GET/POST /trajectories/`, `PATCH/DELETE /trajectories/{name}`, `POST/DELETE /trajectories/{name}/markers`, `GET /trajectory-planner/export`, `GET /trajectory-planner/{skill_name}/export` |
| Teaching | `POST/DELETE/GET /teaching/ghost-objects`, `GET /teaching/ghost-objects/export`, `GET /teaching/ghost-objects/sources`, `GET /teaching/tcps/sources` |
| Szene | `GET/POST /stage/configuration`, `GET/PUT /stage/scene`, `GET /stage/simulation`, `PATCH /stage/simulation/timeline/{action}`, `GET/PUT /stage/units` |
| Sonstiges | `PUT /overlays/robot/visibility`, `GET/PATCH /ui/visibility`, `GET /status`, `GET /version`, Nucleus-Server-Verwaltung |

## Collider lesen: `POST /physics/collision/sweep` (Quellcode + live, 2026-08-20 / 2026-09-09)
- Body `TreeSweepParameters {"sweep_type": "tree", "base_prim_path": "/World/..."}`; Antwort
  `dict[str, Collider]` mit `Collider{shape, pose: nova_models.Pose, prim_path}` — dieselbe
  `Pose`-Klasse (Rotationsvektor) wie im Nova-SDK.
- Query `relative_to_prim=<Roboter-Prim>` liefert Posen im Rahmen dieses Prims statt in
  `/World`-Koordinaten. Das ist die Brücke zur Nova-Kollisionsprüfung, die roboterrelative
  Collider erwartet (verifiziert: 500 mm statt <1 mm Abstand für denselben Scanner).
- Liefert nur **aktivierte** Collider (`physics:collisionEnabled`) mit **exportierbarer**
  Approximation. `Mesh/none` und `Mesh/boundingCube` werden übersprungen, nur als
  `carb.log_warn` in der Isaac-Konsole sichtbar, nicht in der Antwort. Deaktivierte Teilbäume
  (inaktive Prims) fehlen komplett. Mehr-Hüllen-Prims (convexDecomposition) bekommen Keys
  `<prim>/<i>`.
- **Nebenwirkung:** startet die Timeline automatisch, wenn sie steht, und stoppt sie danach
  wieder (`collision_world.py`); ein Sweep kann die Simulation kurz an- und ausschalten. Der
  UI-Export wartet zusätzlich `stabilization_delay` (Default 1 s).
- Roboter-Link-`visuals` sind in gepflegten Szenen absichtlich als Collider deaktiviert; der
  Export überspringt `.../visuals` ohnehin (Nova liefert die Link-Geometrie).
- `PATCH /prims/physics/colliders/` ist NUR Enable/Disable eines Colliders auf einem Prim, kein Listing.

## Weitere Endpunkte
- `GET /stage/motion-groups` bzw. `GET /manipulators/motion-groups`: Roboter-Prim-Pfade der
  Szene (Basis für Cell-Prim-Ableitung und `relative_to_prim`).
- `GET /prims/poses?prim_path=...` → 200 (Pose) / 404 (Prim existiert nicht); brauchbar als
  Existenzprüfung.
- Ein Prim mit fehlerhafter Konfiguration kann HTTP 500 liefern (`'NoneType' ... 'max_shapes'`);
  das ist ein Szenenfehler, kein API-Fehler.

## UI der Extension (Quellcode 2.50.0, 2026-09-09)
- Panel Tools > Wandelbots NOVA > **Collider List**: alle Prims mit `UsdPhysics.CollisionAPI`
  inklusive deaktivierter. Reines UI, kein HTTP-Endpunkt.
- Tab **Collision Setup** (Export): Sweep-Typ `["sphere", "tree"]` (Default `tree` mit
  `base_prim_path`), Referenz-Prim, Werkzeug-Prim (automatisch, wenn genau EIN Prim mit
  `ToolAPI` an die Motion-Group gebunden ist), Self-Collision, Stabilisierungs-Delay,
  "Auto load" (nur Anzeige im Overlay, kein Re-Export). Speichert per
  `StoreCollisionSetupsApi.store_collision_setup`. **Kein API-Endpunkt für den Export selbst**;
  ein Backend baut das Setup aus Sweep + Link-Chain + Tool selbst zusammen (mec_demo ADR 003).
- Panel **Connected Instances**: Eigenschaft `enabled` je Roboter ist der echte Streaming-Schalter
  (autoritativer als `GET /stage/motion-groups`, Lernprozess 2026-08-21).

## Hersteller-Beispiel `examples/nova_sdk/collision_free.py::build_collision_world` (2026-09-09)
Sweep (Kugel r=100) → jeden Collider zellweit `store_collider` (fürs Setup nicht nötig) →
Werkzeug als manuelle Box `store_collision_tool` → Link-Kette `store_collision_link_chain` →
`store_collision_setup` → `get_stored_collision_setup` → `collision_free(collision_setup=...)`.
Werkzeugerkennung dort: `_is_tool_mounted` = Collider im selben Elternordner wie das Roboter-Prim.

## Fallstricke
- **Git Bash mangelt Prim-Pfade:** `/World/...` wird zu `C:/Program Files/Git/World/...`
  umgeschrieben. Anfragen mit Prim-Pfaden aus Python (`requests`) stellen, nicht per `curl` in
  Git Bash.
- Szenen-Inkonsistenz beobachtet: nur `workpiece_01` je Zelle mit `convexHull`, weitere
  Werkstücke mit `boundingCube` (nicht exportierbar). Vor Analysen prüfen, was der Sweep
  überhaupt liefert.
- Die Nova-Kollisionsprüfung prüft Werkzeug- und Werkstück-Collider gegen Roboter-Links; ein
  Collider in Flächenkontakt mit einem Link (Adapterplatte) kann eine Startkollision erzeugen
  (siehe Kontaktband in `nova_sdk.md`). Solche Kontakt-Collider in der Szene deaktivieren.
