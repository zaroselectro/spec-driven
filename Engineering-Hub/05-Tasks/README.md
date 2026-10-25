---
type: index
tags: [tasks, index]
---

# Aufgabenverwaltung

Aufgaben sind Markdown-Dateien mit YAML-Frontmatter. Sie wandern durch vier Ordner – der Ordner **ist** der Status:

```text
backlog/  →  in-progress/  →  review/  →  done/
```

| Ordner | Bedeutung | Eintritt |
| --- | --- | --- |
| `backlog/` | erfasst, noch nicht bearbeitet | Anlage über [[07-Templates/task-template]] |
| `in-progress/` | in Bearbeitung (genau eine verantwortliche Rolle) | Start der Arbeit |
| `review/` | fertig implementiert, Prüfung ausstehend bzw. läuft | Übergabe nach [[04-Workflows/agent-handover]] |
| `done/` | geprüft, Ergebnis angenommen | bestandenes Review, keine offenen kritischen/hohen Befunde |

Rückwärts sind Übergänge erlaubt (`review → in-progress` bei Befunden) und sollten als `updated`-Änderung sichtbar sein.

## Pflichtfelder im Frontmatter

```yaml
id: PROJ-TYP-001
title: Kurztitel der Aufgabe
project: projekt-ordner          # Ordnername unter 03-Projects/ oder "vault"
status: backlog                  # backlog | in-progress | review | done
priority: mittel                 # hoch | mittel | niedrig
assigned_to: backend-developer   # genau eine Rolle aus 01-Agents/
review_by: qa-engineer           # prüfende Rolle (andere als assigned_to)
depends_on: []                   # IDs der Voraufgaben
created: 2026-10-20
updated: 2026-10-20
```

Ergänzend sinnvoll (freiwillig): `tags`, `type`, `mode` (`single-agent` | `multi-agent`).

## Regeln

- **MUSS** Jede Aufgabe hat genau eine `id` (Format: `<PROJEKTKÜRZEL>-<TYP>-<NUMMER>`) und genau eine verantwortliche Rolle in `assigned_to`.
- **MUSS** `review_by` nennt eine **andere** Rolle als `assigned_to`; Selbstabnahme ist kein Review.
- **MUSS** `updated` wird bei jeder Statusänderung angepasst.
- **MUSS** Abhängigkeiten über `depends_on`; keine Aufgabe startet vor ihren Voraufgaben (im Teammodus kontrolliert der Orchestrator).
- **SOLLTE** Eine Aufgabe beschreibt ein Ergebnis, nicht eine Tätigkeitsabsicht („Consumer meldet fehlerhafte Nachrichten in den Fehlerbereich und protokolliert die Ursache“).
- **SOLLTE** Demonstrationsdaten tragen `tags: [demo]` und bleiben in `backlog/`.

## Beispiele (Demonstrationsdaten)

- [[05-Tasks/backlog/DEMO-BE-001]] – Backend-Aufgabe (Kafka-Fehlerbehandlung)
- [[05-Tasks/backlog/DEMO-ARCH-001]] – Architekturaufgabe (ADR)
- [[05-Tasks/backlog/DEMO-OPS-001]] – DevOps-Aufgabe (Auslieferungskonfiguration)

> [!warning] Diese drei Aufgaben sind erfunden, um die Mechanik zu zeigen. Sie sind **keine** realen Anforderungen und werden nicht ohne Weiteres übernommen.

## Verhältnis zu Projekt-Aufgaben

Große oder projektgebundene Aufgaben können in `03-Projects/<projekt>/Tasks/` liegen, sobald sie einen Projektkontext brauchen. Die Felder und der Statusablauf sind identisch. `05-Tasks/` ist die gemeinsame, projektübergreifende Sicht.

## Statuswechsel im Teammodus

Im Multi-Agent-Modus pflegt [[01-Agents/orchestrator]] die Status; Abhängigkeiten und Freigabepunkte: [[04-Workflows/multi-agent-development]]. Im Single-Agent-Modus wechselt die handelnde Rolle den Status selbst und dokumentiert das Review-Ergebnis.

**Siehe auch:** [[07-Templates/task-template]] · [[00-Dashboard/Home]] · [[04-Workflows/agent-handover]]
