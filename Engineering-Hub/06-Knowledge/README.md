---
type: index
tags: [wissen, index]
---

# Wissen

Erfahrungs- und Hintergrundwissen zu den eingesetzten Technologien – **erklärend, nicht verpflichtend**.

## Abgrenzung zu den Standards

| | `02-Standards/` | `06-Knowledge/` |
| --- | --- | --- |
| Zweck | verbindliche/empfohlene Regeln | erklären, einordnen, Einstieg ermöglichen |
| Sprache | **MUSS** / **SOLLTE** / Projektentscheidung | Beschreibung, Hinweise, Stolpersteine |
| Änderung | ADR-fähig, dokumentierte Begründung | jederzeit, wenn das Wissen falsch/veraltet ist |
| Kontextverbrauch | nur die einschlägige Datei | optional; nie notwendig, um Aufgaben zu erledigen |

Was hier steht, ersetzt keine Regel. Widerspricht eine Erklärung einem Standard, gilt der Standard – der Widerspruch wird gemeldet und aufgelöst.

## Bereiche

| Ordner | Thema |
| --- | --- |
| [[06-Knowledge/Java/README]] | Java 21, Sprachumfeld, gängige Fallstricke |
| [[06-Knowledge/Spring-Boot/README]] | Spring-Boot-Konzepte, Laufzeitverhalten, Testarchitektur |
| [[06-Knowledge/Kafka/README]] | Nachrichtenverarbeitung, Liefergarantien, Betrieb |
| [[06-Knowledge/Kubernetes/README]] | Laufzeitumgebung, Manifeste, Betriebsprinzipien |
| [[06-Knowledge/Software-Architecture/README]] | Architekturmuster, ADR-Methode, arc42 |

## Wann lesen?

- Einstieg in eine Technologie, bevor der eigentliche Code gelesen wird.
- Wenn ein Standard unklar wirkt (hier steht oft das *Warum* dahinter).
- **Nicht** bei jeder Aufgabe – Routineaufgaben brauchen nur Standards und Projektregeln (siehe [[04-Workflows/single-agent-development]]).

## Eigenes Wissen ergänzen

Kurze, fokussierte Notiz im passenden Ordner, Verweis auf die zuständige Standarddatei, Kennzeichnung als Erfahrungswissen (Datum). Keine Regeln, keine erfundenen Projektangaben.

**Siehe auch:** [[02-Standards/README]] · [[00-Dashboard/Home]]
