# Domänenwissen — Index (geteilt)

> Projektunabhängige, verifizierte Fakten. Jede Aussage trägt "verifiziert (wie/wann)" oder
> "Annahme". Projektfakten (Pfade, Ports, Prims, Posen) gehören ins Projekt, nicht hierher.

| Datei | Inhalt |
|---|---|
| [terminologie.md](terminologie.md) | Kanonisches Domänen-Glossar (Collider, Setup, Margins/Luft, Kollisionsarten, Flansch/TCP, Halt/Stop/Pause) und Invarianten |
| [nova_sdk.md](nova_sdk.md) | Wandelbots Nova SDK und API: Kollisions-API, Planen ohne Bewegung, Fehlerarten, Kontaktband, Importfalle, Motion-Group-Claim, Typen, NATS |
| [isaac_omniservice.md](isaac_omniservice.md) | Isaac Sim mit Wandelbots-Extension: OmniService-Endpunkte, Sweep-Verhalten, `relative_to_prim`, Collider List, Export-Dialog, Fallstricke |
| [novaflow_referenz.md](novaflow_referenz.md) | wandelbotsgmbh/novaflow (privat): ADR-Praxis und Terminologie als Vorbild, dort gemessene Nova-Fakten |
| [claude_code_umgebung.md](claude_code_umgebung.md) | Claude Code und VS Code: CLAUDE.md-Importe, Memory-Mechanik und Verluste, Kontextfenster, Keybindings, Git-Zugriffe, Shell-Fallstricke; Account-Kontext |

## Übergreifende Regeln für dieses Verzeichnis
- Wenn ein Nova-Plattform-Fakt gebraucht wird: erst hier und in `novaflow_referenz.md` nachsehen,
  ob er schon gemessen wurde, bevor angenommen wird.
- Neue Fakten mit Datum und Methode eintragen; widerlegte Fakten nicht löschen, sondern als
  "widerlegt (wann/wie)" markieren, damit die Entscheidung nicht erneut aufgerollt wird.
