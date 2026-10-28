# Skills im Engineering-Hub

Skills beschreiben wiederverwendbare Fähigkeiten mit einem nachvollziehbaren Ergebnis. Der Hub unterscheidet zwei Ebenen: fachliche Methoden für Menschen und Agenten sowie Agent-Skills als ladbare Pakete mit `SKILL.md`.

## Einordnung

| Baustein | Leitfrage | Beispiel |
| --- | --- | --- |
| Rolle (`01-Agents`) | Aus welcher Verantwortung arbeite ich? | Anforderungsanalyst |
| Standard (`02-Standards`) | Welche Regeln gelten? | REST-Standards |
| Workflow (`04-Workflows`) | Wie läuft die Gesamtaufgabe ab? | Feature-Entwicklung |
| Methode / Skill (`10-Skills`) | Welche Fähigkeit liefert ein bestimmtes Ergebnis? | prüfbare Akzeptanzkriterien formulieren |
| Agent-Skill (`.agents/skills`) | Welche Anleitung lädt der Agent für diese Tätigkeit? | `hub-requirements-analysis/SKILL.md` |
| Vorlage (`07-Templates`) | Wie wird das Ergebnis strukturiert? | Feature-Vorlage |
| Tool / MCP | Womit werden Daten gelesen oder Aktionen ausgeführt? | Repository-Dateien lesen |

Eine Rolle kann mehrere Skills nutzen; derselbe Skill kann mehreren Rollen dienen. Ein Agent-Skill ist eine Anleitung mit optionalen Hilfsmitteln, kein eigenständiger Prozess. Er erweitert keine Berechtigungen und startet kein Team.

## Einstieg

- [[10-Skills/skill-catalog]]: Methoden, passende Rollen, Ergebnisse und verfügbare Pakete.
- [[10-Skills/Development/implement-from-spec]]: vollständiger Beispiel-Skill mit fünf Umsetzungsschritten.
- [[10-Skills/agent-skills]]: Format, Erkennung, Nutzung und Übernahme in andere Projekte.
- [[07-Templates/skill-profile-template]]: eine Methode beschreiben.
- [[07-Templates/agent-skill-template]]: daraus ein Agent-Skill-Paket erstellen.

## Fachliche Bereiche

- **Requirements:** [[10-Skills/Requirements/hub-requirements-analysis|Anforderungsanalyse]]
- **Development:** [[10-Skills/Development/implement-from-spec|Spezifikation implementieren]]
- **Quality:** [[10-Skills/Quality/hub-code-review|Code-Review]]

Die sichtbaren Lesefassungen sind fachlich gegliedert. Die Agent-Pakete bleiben unter `.agents/skills/<skill-name>/` abgelegt.

## Pflege

Fachliche Methoden stehen im Katalog. Operative Anweisungen der umgesetzten Skills werden in deren `SKILL.md` gepflegt. Für Obsidian gibt es vollständige Lesefassungen unter `10-Skills`; der Katalog verlinkt auf diese sichtbaren Notizen. Nach Änderungen an einer Quelldatei die entsprechende Lesefassung aktualisieren. Der versteckte Ordner `.agents` wird nicht als regulärer Obsidian-Notizordner vorausgesetzt. Technologie- und Qualitätsregeln bleiben unter `02-Standards`, Projektentscheidungen im jeweiligen Projekt. Skills lesen nur die zur Aufgabe passenden Quellen.

Für einen neuen Skill zuerst Auslöser, Eingaben, Ergebnis und Qualitätskriterien festlegen. Ein Paket lohnt sich, wenn wiederkehrende Arbeit von zusätzlichen konkreten Anweisungen profitiert. Eine gewöhnliche Aufgabe benötigt nicht automatisch einen eigenen Skill.

Änderungen an Paketen anhand eines passenden Auftrags und eines ähnlich klingenden, unpassenden Auftrags prüfen. Bei fehlenden Eingaben muss das Ergebnis offene Punkte zeigen. Eine Strukturprüfung belegt keine erfolgreiche Ausführung in Codex.
