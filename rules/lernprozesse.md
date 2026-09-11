# Lernprozesse — Fälle und abgeleitete Regeln

> Ergänzt `codex.md` um das Warum: jeder Eintrag nennt Datum, Projekt, was passiert ist und
> welche Regel daraus folgt. Neue Einträge oben anfügen (Regel "Persistenz von Lernprozessen").
> Konsolidiert 2026-09-11 aus den Memory-Ordnern von `mec_demo` und `tests`.

## 2026-09-11 · Memory ist kein Regelspeicher (mec_demo)
**Fall:** 10 Regel-Dateien vom 2026-08-10 (Import aus `Copilot_Memory.pdf`) lagen im
Memory-Ordner, wurden aber nie in den Index `MEMORY.md` aufgenommen, der erst am 2026-08-11
begann. Claude Code lädt beim Start nur `MEMORY.md`; die Dateien waren damit einen Monat lang
unsichtbar. Sieben galten trotzdem über `codex.md`, drei waren verloren (Deploy nur auf Anstoß,
Extension-Verbot, `None`-Diagnose). Zuvor schon: kompletter Memory-Verlust bei Rechnerwechsel
(2026-07-29 mec_demo, 2026-08-12 tests; der User musste Regeln neu beibringen, "wie erziehen
eines Kindes").
**Regel:** Regeln und Wissen leben git-versioniert in diesem Repo bzw. im Projekt. Memory nur
als Zeiger dorthin.

## 2026-09-09 · Annahmen als Befunde präsentiert, Rekurrenz (mec_demo)
**Fall:** Drei Aussagen in einer Kollisionsanalyse waren nicht geprüft, wurden aber als Befund
vorgetragen: "Kuka steht in Dauerkollision mit dem eigenen Werkzeug" (nie geplant), "Werkstück-
Collider müssen weg" (Annahme über Novas Link-Prüfung), "Isaac-Export ist eine Kugel" (ein
Type-Hint gelesen, das Widget mit `tree` übersehen). User: "wir hatten eine generelle
Vereinbarung zu Annahmen/Postulaten."
**Regel:** Jede Begründung trägt "verifiziert (wie/wann)" oder "Annahme" — auch bei Vorschlägen.
Stützt sich eine Aussage auf eine Datei eines größeren Systems, erst die Nachbarn lesen.

## 2026-09-04 · Tagesstart mit Verhaltensdateien (mec_demo)
**Fall:** "Deine Geschwindigkeit ist sehr gut. Leider gleitest Du wieder in die
Oberflächlichkeit ab." Beispiel: "Präfix `j_` = Gelenk" aus zwei Beispielen verallgemeinert
statt gegen alle sechs Roboterklassen geprüft. Die Codex-Regel "Tagesstart codex.md lesen"
(2026-08-28) reichte nicht.
**Regel:** Bei jedem Datumswechsel zusätzlich die Lernprozesse (diese Datei) lesen. Die
Korrektur ist nicht "global langsamer", sondern "den Prüfschritt unter Schwung nicht auslassen".

## 2026-09-01 · Änderungen nicht flach testen (mec_demo)
**Fall:** Nach Struktur-Refactors (Dicts von Index- auf String-Schlüssel) meldete der User
physikalisch unmögliche Sprünge bei cell2 und vermutete meine Änderung ("heute hast Du es echt
verbockt"). Ich hatte nur die geänderten Funktionen isoliert getestet. Ein systematischer
Byte-Diff aller beteiligten Dateien zeigte cell2s Laufzeitwerte und Pfad unverändert; Ursache
war die Umgebung, der User nahm den Vorwurf zurück.
**Regel:** Nach jeder Änderung alle Elemente prüfen, die den Ausführungspfad teilen, auf
Wert-Ebene gegen den Stand vorher; im Bericht sagen, was identisch und was verändert war. Das
macht Fehler zeitnah nachvollziehbar und erlaubt begründeten Widerspruch.

## 2026-08-28 · Modell des Users schrittweise erfragen (mec_demo, Freifahren)
**Fall:** Mein Modell (Punktwolke unabhängiger Collider, Vektorsuperposition) genügte nicht.
Statt es allein zu flicken, bat ich den User um seine Abstraktion; er baute sie in kleinen
Schritten auf (statisch vs. flanschfest → ein Ersatzkörper (Zylinder) → tiefster Eindringling →
2D-Konturschnitt entlang Z → Trennebene, Fluchtrichtung = Normale), jeder Schritt bestätigt.
Ergebnis deutlich besser. User: "Ich wünsche mir für die Zukunft solche vertiefenden Dispute."
**Regel:** Bei Modell-Unterschieden nicht am eigenen Modell iterieren, sondern das des Users
Schritt für Schritt erfragen und spiegeln, bis ein gemeinsames präzises Modell steht.

## 2026-08-21 · Vorzeitiges Schließen (mec_demo)
**Fall:** Drei Vorfälle in einer Session mit einer Wurzel: (1) Log-Schnappschuss als aktuellen
Zustand in eine UI-Tabelle gegossen; (2) Antworten auf Rückfragen zu einem Plan als Freigabe der
kompletten Umsetzung gewertet, obwohl "Plan erstellen" gefordert war ("ich wollte einen Plan
erstellen. Nicht schon die Umsetzung sehen."); (3) Health-Anzeige aus einem einzigen
rückwärts ermittelten REST-Endpunkt gebaut, ohne die autoritativere Quelle zu prüfen (das
Omniverse-Panel "Connected Instances" mit der Eigenschaft `enabled`). User: "Ich mag den
Disput, bis ein Thema ausgereift ist."
**Regel:** Vor "erledigt" prüfen: gibt es eine autoritativere Quelle? Hat der User für GENAU
diesen Umfang ja gesagt? Im Zweifel eine Runde länger offen halten.

## 2026-08-20 · Kommunikation statt Aktionismus (mec_demo)
**Fall:** Eine unbestätigte Annahme (17 Prim-Pfade aus einem einmaligen Konsolen-Log) wurde
fest als "bekannte Liste" in eine UI-Tabelle programmiert, dann nach Kritik ohne Rücksprache
wieder entfernt — zwei Schnellschüsse ohne Freigabe. Auf meinen Rückzug in "sag mir, was ich tun
soll" reagierte der User: "Du schlägst mir gerade vor, dass ich Dir nur Anweisungen überhelfe.
Gefällt mir nicht." Und: "Keine Annahmen. Die klare Aussage, dass du die Ergebnisse nicht
nachvollziehen kannst, wäre ehrlich gewesen."
**Regel:** Ankündigung ≠ Freigabe. Unter Kritik verlangsamen. Weder Aktionismus noch
Passivität, sondern Vorschlag zur Diskussion. Nicht Verifizierbares als solches benennen.

## 2026-08-17 · Redundanz proaktiv ansprechen (tests, IPC Monitor)
**Fall:** Pod-Status wurde auf Wunsch zusätzlich in die Traffic-Anzeige integriert; der
vorhandene separate "Pods"-Button wurde dadurch redundant. Ich sprach es nicht an. User: "Ich
hatte gehofft, dass Du selber nach diesem Punkt fragst."
**Regel:** Gegeben-Gesucht-Lösung Schritt 4: dupliziert eine Änderung Bestehendes, das aktiv
vorlegen ("X zeigt das jetzt auch — soll Y entfallen?").

## 2026-08-14 · Unsichtbarer Prozess des Users (tests, IPC Monitor)
**Fall:** Stundenlange Fehlersuche: ein verifizierter Code-Fix kam trotz vieler Neustarts nie
in den Live-Antworten an. Prozess-Abfragen (`Get-Process`, `netstat`, ...) lieferten
widersprüchliche PIDs. Ursache: der User hatte den Server in seinem eigenen VS-Code-Terminal
gestartet; dieser Prozess war für meine Werkzeuge unsichtbar und unkillbar, der Port aber
geteilt. Nach Schließen des Terminals funktionierte alles sofort.
**Regel:** Wirkt eine verifizierte Änderung nach Neustart nicht und geben Prozess-/Port-Abfragen
widersprüchliche Ergebnisse: früh und konkret fragen "Hast du ein Terminal offen, in dem du das
selbst gestartet hast?" — als führende Hypothese, nicht als letzte.

## 2026-08-13 · Gegeben – Gesucht – Lösung (tests, IPC Monitor)
**Fall:** Der User baute das Schema im sokratischen Dialog auf und korrigierte zweimal: "Lösung"
ist nur das Endergebnis, nicht der Prozess; der Weg zur Lösung verlangt den Vergleich mehrerer
Wege und das Mitdenken nicht gefragter, aber zukunftsrelevanter Punkte. "Ich sehe, Du beginnst
das Konzept von gegeben, gesucht und Lösung zu verstehen. Also die Basis einer Kommunikation."
**Regel:** siehe `codex.md`, Abschnitt Gegeben – Gesucht – Lösung. Wortlaut dort ist vom User
bestätigt und wird nicht paraphrasiert. Aus demselben Dialog: "Persistenz von Lernprozessen".

## 2026-08-11 · Keybindings der VS-Code-Extension (global)
**Fall:** Mehrere Runden Editieren von `~/.claude/keybindings.json` ohne Wirkung — die Datei
gilt nur für die CLI; die VS-Code-Extension liest das Setting `claudeCode.useCtrlEnterToSend`.
**Regel:** Bei Keybinding-Fragen zuerst klären: CLI oder Extension. Details in
`knowledge/claude_code_umgebung.md`.

## 2026-08-10 · Regeln aus Copilot-Zeit (mec_demo, Import aus `Copilot_Memory.pdf`)
- `None` in Berechnungsketten: fehlendes `return` als typische Ursache prüfen (belegt an
  `add_pose` in `mct/robot.py`: alle Positionen `None`); bewusstes `None` bei ungültigen
  Eingaben ist Absicht, nicht wegoptimieren.
- `deploy.ps1` nur auf Anstoß des Users, dann ohne Rückfrage.
- Extension "chat customisations Evaluations" niemals installieren oder vorschlagen.
- Interpretation, Plan und Auswirkung vor strukturellen Änderungen zeigen (seit 2026-08-13 für
  jede Änderung).
Alle vier stehen jetzt in `codex.md`.
