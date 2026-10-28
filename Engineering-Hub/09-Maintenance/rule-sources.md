# Regelquellen und Auswahl

## Zuständigkeiten
| Quelle | Verantwortlichkeit |
| --- | --- |
| `02-Standards/` | Allgemeine Technologie- und Qualitätsregeln für unterschiedliche Projekte |
| `specs/base/` | Explizit gewählte Spring-Boot-Projektbasis, Architektur und Arbeitsablauf |
| `specs/extensions/` | Nur ausdrücklich aktivierte Erweiterungen mit Voraussetzungen |
| Projektanforderungen und akzeptierte ADRs | Konkrete Fachlichkeit, Architekturentscheidungen und begründete Abweichungen |
| `06-Knowledge/` | Hintergrundwissen ohne verbindliche Regelwirkung |
| `10-Skills/` und `.agents/skills/` | Methoden-Katalog und operative Skill-Anleitungen; verweisen auf zuständige Standards und Projektentscheidungen |

## Auswahl und Konflikte
Laufzeitvorgaben und ausdrückliche Nutzeraufträge beachten. Die Spring-Boot-Basis gilt nur nach bewusster Übernahme in ein Projekt. Ihr Vorhandensein im Hub aktiviert nichts. Ohne diese Auswahl bleiben beispielsweise Lombok und Liquibase Projektentscheidungen. Bei gewählter Basis gelten die dort definierten Anforderungen; Erweiterungen ausschließlich gemäß aktivierter Liste und Voraussetzungen in `../../../project.md` anwenden.

Konkretere Regeln dürfen allgemeine Vorgaben präzisieren. Bei echten Widersprüchen die betroffenen Regeln benennen und vor der betroffenen Umsetzung eine dokumentierte Entscheidung herbeiführen; keine automatische Rangfolge allein aufgrund von Dateinamen annehmen. Bereits bestätigte Entscheidungen nicht erneut abfragen. ADRs müssen Abweichung und Geltungsbereich ausdrücklich nennen.

## Bewusste Zusammenführung
Die allgemeinen Standards stammen aus `software-engineering`, die detaillierten optionalen Spring-Boot-Regeln aus `mimo/specs`. Allgemeine Regeln für Polling, Monitoring, Compose, Importe und Datenbanktests werden unter `02-Standards/` gepflegt. Die zugehörigen Spring-Boot-Erweiterungen binden diese ausdrücklich ein und ergänzen nur ihre Basisanbindung. Die Übernahme umfasst die referenzierten Standards und deren dokumentierte Voraussetzungen; die Kopierzuordnung steht in `specs/README.md`.

Doppelte technische Vorschriften aus den `mimo`-Standards wurden dadurch nicht erneut kopiert. Die Specs in `foo` und `mimo` sind bytegleich. Die kompakten `foo`-Rollen und -Workflows wurden durch die ausführlicheren Fassungen aus `mimo` abgedeckt; sie bilden keine zweite aktive Regelsammlung.

Keine automatische Aktualisierung der Ursprungsordner oder bestehender Projekte. Künftige Änderungen gezielt hier pflegen. Ein Projekt-Snapshot muss Quelle, Datum, Dateiliste und einen überprüfbaren Dateistand festhalten; solange kein Commit die Inhalte enthält, keinen Release- oder Commitstand behaupten.
