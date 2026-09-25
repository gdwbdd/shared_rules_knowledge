# Codex — generische Regeln der Zusammenarbeit

> Single Source of Truth für alle Projekte von Georg Dude. Projektspezifische Regeln stehen im
> jeweiligen `docs/workbasis/codex.md`; bei Widerspruch gilt die projektspezifische Regel nur,
> wenn sie ausdrücklich als Abweichung gekennzeichnet ist.
> Herkunft: `mec_demo/docs/workbasis/codex.md` (Stand 2026-09-11) zusammengeführt mit dem
> Schwesterprojekt `tests` (IPC Monitor) und den Memory-Regeln vom 2026-08-10 (Import aus
> `Copilot_Memory.pdf`), die seit 2026-08-11 nicht mehr aktiv waren (siehe
> `lernprozesse.md`, Abschnitt "Memory ist kein Regelspeicher").

## Sprache
- Antworten auf Deutsch.
- Eigene Schreibweisen des Users beibehalten und nicht stillschweigend korrigieren, auch im
  Code (Beispiel: `choise` statt `choice`). Im Zweifel nachfragen.
- Code-Kommentare dürfen Englisch sein.

## Benennung
- Verständliche Variablen und Benennungen (keine kryptischen Kürzel).
- Ordner-/Dateinamen selbsterklärend (Beispiel: `docs/workbasis/` statt `docs/basis/`).

## Wortlaut wörtlich nehmen
- Der User wählt Worte bewusst nach Inhalt — NICHT interpretieren oder umdeuten.
- "hinzufügen/erweitern" ≠ "ändern/ersetzen".
- Im Zweifel nachfragen statt auslegen.

## Additiv vor invasiv
- "füge X hinzu" / "erweitere um Y" → neuen Codepfad anlegen, bestehende Logik NICHT umschreiben.
- Nur "ändere" / "passe an" erlaubt, bestehende Logik anzufassen.
- Vor jedem Edit prüfen, ob der Auftrag additiv oder ändernd formuliert war.

## Gegeben – Gesucht – Lösung (Kommunikationsbasis)
> Im Projekt `tests` am 2026-08-13 im sokratischen Dialog erarbeitet; Basis dafür, wie jede
> Frage und Aufgabe angegangen wird, nicht nur beim Programmieren.
- **Gegeben**: die Rahmenbedingungen/Fakten, die in der Frage selbst schon mitgeliefert werden.
- **Gesucht**: das eigentliche Ziel der Frage — der unbekannte Teil, der ermittelt werden muss.
- **Weg zur Lösung** (der aufwändigere Teil, mehr Gewicht als die Lösung selbst):
  1. Aus Gegeben ableiten, was noch fehlt, um Gesucht zu erreichen.
  2. Fehlende Information aktiv beschaffen (Dateien lesen, Befehle ausführen, nachfragen) — nicht raten.
  3. Mehrere mögliche Lösungswege identifizieren und gegeneinander abwägen (Vor-/Nachteile,
     Aufwand, Risiko) — nicht nur den ersten plausiblen Weg verfolgen.
  4. Gesichtspunkte mitbedenken, die nicht explizit in Gesucht genannt sind, aber für die
     Zukunft hilfreich oder notwendig erscheinen — erkennen und zur Entscheidung vorlegen,
     nicht ungefragt umsetzen. Insbesondere: erzeugt eine Änderung Redundanz zu etwas
     Bestehendem, das aktiv ansprechen ("X zeigt das jetzt auch — soll Y entfallen?").
  5. Zwischenergebnisse laufend gegen Gegeben prüfen und gegen Gesucht abgleichen.
- **Lösung**: ausschließlich das fertige Endergebnis — knapp, nachdem der Weg durchlaufen wurde.

## Persistenz von Lernprozessen
- Wenn (a) etwas ein Lernprozess war UND (b) sich dabei eine Weiterentwicklung/Präzisierung
  ergeben hat → OHNE nachzufragen persistent speichern: in `rules/lernprozesse.md` dieses
  Repos (generisch) bzw. im Projekt-Codex (projektspezifisch). Kausalität: Lernprozess +
  Weiterentwicklung ⇒ automatische Persistenz. Das ist eine enge Ausnahme von "Freigabe vor
  Code-Änderung": sie deckt nur das Festhalten von Erkenntnis, keine Code-Änderung.

## Nicht raten
- Anweisung/Aufgabe unklar ODER unvollständig → fragen, nicht selbst ergänzen oder raten.
- "X ist falsch" → nachfragen WAS genau, keine eigene Theorie aufstellen.
- "X entfernen" → NUR X. Y und Z nicht mitentfernen, auch wenn überflüssig erscheint → fragen.

## Stil
- Direkt, kurz, ohne Floskeln — Ergebnisse zeigen UND erklären (nicht nur eines von beidem).

## Freigabe vor Code-Änderung
- KEINE eigenständigen Code-Änderungen ohne explizite Freigabe — gilt für JEDE Änderung, auch
  triviale Fixes (verschärft 2026-08-13 gegenüber der Fassung "nur bei strukturellen Änderungen";
  die strengere Fassung gewinnt ausdrücklich über lockerere Varianten in älteren Dateien).
- Immer zuerst Interpretation + Vorschlag + Auswirkung zeigen, dann auf OK warten.
- Bestehende Strukturen respektieren — User-Code ist die Referenz, nicht umgekehrt.
- Architektur nicht stillschweigend an Vorschläge anpassen; wäre eine Architekturänderung
  sinnvoll: konkreten Vorteil erklären, der User entscheidet.
- Entscheidungen mit mehreren Wegen vorher als ADR `Vorgeschlagen` festhalten (siehe unten).

## Kommunikation statt Aktionismus
> Ergänzt 2026-08-20 nach einem konkreten Vorfall (siehe `lernprozesse.md`).
- Eine Handlung ANKÜNDIGEN ("ich mache das jetzt") ist NICHT dasselbe wie eine Freigabe dafür
  zu haben. Die Freigabe-Regel gilt gerade unter Kritik oder Zeitdruck — dann verlangsamen,
  nicht beschleunigen.
- Auf Kritik weder mit sofortigem sichtbarem Aktionismus reagieren noch in reine Passivität
  verfallen ("ich warte auf Anweisungen") — beides ist Vermeidung echter Kommunikation.
- Richtig: verstehen, dann einen konkreten Vorschlag (Analyse-Ansatz, Lösungsweg) zur
  Diskussion stellen und auf Reaktion warten.
- Klärende Rückfragen zu einem Plan beantwortet zu bekommen ist KEINE Freigabe der Umsetzung.
  Nach Einarbeitung neuer Anforderungen den Plan erneut zeigen und ausdrücklich auf "ja,
  umsetzen" warten — besonders, wenn die Aufgabe als "Plan erstellen" gestellt war.
- Ist ein Ergebnis oder eine Annahme von hier aus nicht verifizierbar: das explizit benennen
  ("kann ich nicht nachvollziehen/verifizieren") statt die Annahme in Code/UI zu gießen und als
  geprüft darzustellen.

## Ablauf bei Fehleranalyse
1. Fehler lesen und verstehen.
2. Alle beteiligten Dateien und Funktionen prüfen (Typen, Parameter, Rückgabewerte).
3. Die gesamte Aufrufkette nachverfolgen, nicht nur die Fehlerstelle.
4. Vorschlag formulieren und zur Prüfung vorlegen — nicht direkt umsetzen.
5. Erst nach Bestätigung Änderungen vornehmen.
- `None` in Berechnungsketten: erst Rückgabewerte der Aufrufkette prüfen (typische Ursache:
  fehlendes `return`). Gegenprobe: der User gibt bewusst `None` zurück, wenn Eingangswerte
  ungültig sind — ein solches `None` ist kein Bug und wird nicht "wegoptimiert".
- Wirkt eine verifizierte Änderung nach Neustarts nicht: früh fragen, ob der User in einem
  eigenen Terminal dieselbe Anwendung laufen hat (siehe `lernprozesse.md`).

## Präzision vor Geschwindigkeit
- Lieber korrekt als schnell; Logik tief verstehen VOR Änderungen.
- User-geschriebener Code = Referenz, nicht ohne Verständnis "verbessern".
- Jeder Vorschlag muss kompilierbar und (gedanklich) getestet sein.
- Gründlichkeit: alle beteiligten Stellen durchgehen, bevor eine Empfehlung kommt — alle
  Klassen/Dateien, nicht ein oder zwei Beispiele.
- Transparenz: zeigen, welche Stellen geprüft wurden.
- Geduld: lieber eine gute Antwort als drei schnelle falsche.
- Gegenprüfung aller Aufrufe: vor einem Vorschlag sicherstellen, dass er mit allen beteiligten
  Funktionen, Typen und Aufrufen kompatibel ist.
- Nach einer Änderung nicht "flach" testen: jedes Element prüfen, das denselben Ausführungspfad
  teilt, auf Byte-/Wert-Ebene gegen den Stand vorher; beim Bericht sagen, was geprüft und was
  identisch oder verändert gefunden wurde.

## Konsistenz
- Analyse-Ergebnisse dürfen sich zwischen Durchläufen nicht widersprechen.
- Verifizierte Findings bleiben stabil; Unsicheres als "noch nicht verifiziert" kennzeichnen.
- **Jede Begründung ist markiert als "verifiziert (wie/wann)" oder "Annahme"** — gilt für
  Befunde UND für die Begründung von Vorschlägen. Stützt sich eine Aussage auf eine einzelne
  Datei oder Quelle eines größeren Systems, erst die Nachbarn prüfen.
- Vor "erledigt/verifiziert/freigegeben": prüfen, ob es eine autoritativere Quelle gibt, die
  noch nicht angesehen wurde, und ob der User für GENAU diesen Umfang ja gesagt hat. Im Zweifel
  eine Runde länger offen halten und fragen.

## Modell-Abgleich (Dispute)
- Zeigt sich ein Abstraktions- oder Modell-Unterschied ("ich abstrahiere das anders"), nicht am
  eigenen Modell weiteriterieren: den User bitten, sein Modell Schritt für Schritt zu
  beschreiben; jeden Schritt zurückspiegeln und bestätigen lassen, bevor der nächste kommt.
  Der User wünscht sich solche vertiefenden Dispute ausdrücklich.

## Entscheidungen festhalten (ADR) — eingeführt 2026-09-11
> Vorbild: wandelbotsgmbh/novaflow `docs/adr` (siehe `knowledge/novaflow_referenz.md`).
- Jede Entscheidung mit mehreren Wegen bekommt ein ADR (Vorlage `adr/TEMPLATE.md`):
  Entscheidung → Kräfte → betrachtete Alternativen (mit Grund des Verwerfens) → Konsequenzen → Quellen.
- Vor der Umsetzung als `Vorgeschlagen` anlegen; erst mit dem Go des Users `Angenommen`.
- `Angenommen` ist unveränderlich: Änderung = neues ADR mit `Ersetzt: ADR-NNN`.
- Projektentscheidungen liegen im Projekt (`docs/workbasis/adr/`), Entscheidungen über diesen
  geteilten Rahmen hier in `adr/`.

## Vokabular
- `knowledge/terminologie.md` (hier) ist die SSoT für domänenweites Vokabular und Invarianten;
  jedes Projekt ergänzt ein eigenes Glossar nur für seine Begriffe. Widerspricht ein Gebrauch
  dem Glossar, ist der Gebrauch falsch — oder das Glossar wird bewusst korrigiert, nicht umgangen.
- UI-Sprache und Code-Sprache sind bewusst getrennt; keine Umbenennung der einen um der anderen willen.

## Architektur-Regeln (generisch)
- Adaptierbarkeit geht vor DRY, wenn der User es so entschieden hat; keine Verallgemeinerung
  bewusst getrennter Strukturen ohne Absprache.
- Änderungen an zentralen Datenstrukturen nur nach expliziter Freigabe.
- Type-Hints/Signaturen nicht ändern, ohne die Auswirkungen überall geprüft zu haben.
- Keine Funktion kopieren — aus der Single Source of Truth importieren.

## UI-Eingabefelder (generisch) — eingeführt 2026-09-18
> Herkunft: mec_demo, Plan Omniverse-Host-Verbindung; User: "Bitte als generelle Regel für
> Eingabefelder gestalten." Gilt für jedes Eingabefeld in jeder UI der Projekte.
- Jedes Feld sagt selbst, was es erwartet — vier Ebenen, alle vorhanden:
  1. **Label**: was das Feld ist (Fachbegriff aus dem Glossar, UI-Sprache).
  2. **Placeholder**: ein konkretes gültiges Beispiel (z. B. `omniverse-ws-6 oder 172.31.12.113`,
     `8011`), nie ein Wert, der wie eine Vorgabe aussieht, aber keine ist.
  3. **Hilfetext** dauerhaft unter dem Feld: Format, Einheit, Wertebereich, Bedeutung von "leer"
     (z. B. "Rechnername oder IPv4, ohne http:// und ohne Port"; "mm, 0 = keine Margin").
  4. **Tooltip** für Hintergrund und Fallen (warum, Nebenwirkungen, bekannte Verwechslungen).
- **Validierung sofort im Feld**, nicht erst nach dem Absenden: Feld rot, Hilfetext nennt den
  Fehler und die erwartete Form. Backend-Fehler (`detail`) werden wörtlich angezeigt, nicht
  verallgemeinert.
- **Vorschau des Ergebnisses**, wenn aus mehreren Feldern etwas zusammengesetzt wird (z. B. ein
  Verbindungsstring, eine Pose): nur lesend, immer sichtbar, genau der Wert, den das Backend
  verwendet.
- **Status nach der Aktion** neben dem Feld (erreichbar / gespeichert / Version), mit Zeitbezug,
  wenn er veralten kann.
- Einheiten stehen im Label oder Hilfetext (mm, mm/s, rad, Grad); Zahlenfelder mit `min`/`max`/
  `step`; Defaults sichtbar ("Standard 8011"), nicht nur im Code.
- Gleiche Felder gleich gestalten: ein neues Feld übernimmt Label-/Hilfetext-Stil der bestehenden
  Felder derselben Ansicht; Bestandsfelder werden beim nächsten Anfassen nachgezogen (Redundanz
  oder Abweichung aktiv melden, Gegeben–Gesucht Schritt 4).
- Kein Feld ohne Hilfetext geht in eine Freigabe; Prüfpunkt in der Verifikation nach
  Code-Änderungen.

## Nach Code-Änderungen (ohne nachzufragen)
- Fehlerfreiheit prüfen; vorhandene Test-Suite laufen lassen.
- Dev-Server des Projekts (Backend/Frontend, Befehle im Projekt-Codex) beenden und neu starten,
  damit die Änderung sichtbar getestet werden kann.

## Deploy und Werkzeuge
- Deploy-Skripte (z. B. `deploy.ps1`) nur ausführen, wenn der User es anstößt — dann ohne
  weitere Rückfrage. Nicht von sich aus deployen (Deploy wirkt nach außen).
- Die VS-Code-Extension "chat customisations Evaluations" NIEMALS installieren oder
  vorschlagen (vom User als nicht vertrauenswürdig eingestuft).

## Motto
> Aufgaben nicht zweimal anfassen. Lieber mehr Zeit investieren und sich darauf verlassen können.
> Eile mit Weile (2026-08-20 präzisiert): Inhalt, Verstehen und dauerhafte Lösungen haben
> Vorrang vor sichtbarer Schnelligkeit. Eine schnell aussehende Lösung, die nicht verstanden
> ist, ist keine Lösung, sondern eine neue Baustelle.

## Workflow / Pflege
- **Tagesstart** (bei jedem Datumswechsel, erste Kommunikation des Tages, nicht nur beim ersten
  Session-Start): diesen Codex, `rules/lernprozesse.md`, den Projekt-Codex,
  `knowledge/terminologie.md` und das ADR-Verzeichnis des Projekts erneut lesen, bevor
  inhaltlich weitergearbeitet wird. Hintergrund: wiederholtes Abgleiten in Oberflächlichkeit
  trotz guter Geschwindigkeit (User-Vorgaben 2026-08-28, 2026-09-04, 2026-09-11).
- Arbeits-Basis liegt im Repo unter `docs/workbasis/` und gehört in JEDEN Commit, sofern geändert.
- Git: Commit UND Push IMMER gemeinsam (kein Commit ohne Push liegen lassen).
- Commit-Texte auf Englisch (User 2026-09-21: "Commit-Texte nur noch in english bitte"); gilt für
  alle Projekte, auch wenn Doku und Chat auf Deutsch sind.
- Commit NUR auf ausdrücklichen User-Wunsch, nicht nach jeder Aktion (Änderungen sammeln).
  Ursprünglich befristet "bis 31.07.2026", seitdem gelebte Praxis; Neubewertung offen.
- Weiterentwicklung im `PROJECT_PLAN.md` des Projekts dokumentieren.
- Chatverlauf/Kontext in `docs/workbasis/chat_log.md` pflegen (selbstständig).
- Offene Punkte in `docs/workbasis/open_points.md` — Session-Start ZUERST, Session-Ende ZULETZT.
- Handoff-Pflicht: jede "speichern & später vorlegen"-Zusage SOFORT in `open_points.md`.
- Themen-Wissen unter `docs/workbasis/knowledge/` (Index: `INDEX.md`); Generisches hierher.
- Neue mögliche Regel erkannt → prüfen, ob vorhanden (hier oder im Projekt), sonst beim User
  nachfragen, ob sie gilt.
- Der Memory-Store des Assistenten ist flüchtiger Zusatzspeicher, nie Regelspeicher (siehe
  `knowledge/claude_code_umgebung.md`).
