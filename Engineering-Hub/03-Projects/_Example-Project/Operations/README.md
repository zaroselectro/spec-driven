---
type: placeholder
tags: [projekt, operations]
---

# Operations – _Example-Project

**Bewusst vorgesehener Bereich für spätere Inhalte.** Auslieferungs- und Betriebsunterlagen des Projekts entstehen hier.

## Was hierher gehört

- Container- und Auslieferungsbeschreibung (nach [[02-Standards/Infrastructure/docker-standards]]).
- Laufzeitmanifeste bzw. die Konfiguration derselben (nach [[02-Standards/Infrastructure/kubernetes-standards]]).
- Konfigurationsmatrix: Variable, Zweck, Quelle, Standard, ob geheim.
- Umgebungsbeschreibung und Reihenfolge der Auslieferungsschritte inklusive Rollback.
- Hinweise zu Monitoring, Logging und Alarmierung (was wird beobachtet, was löst Alarm aus?).

## Regeln

- **MUSS** Keine Geheimnisse in diesem Ordner – Quellen benennen, Werte niemals aufschreiben.
- **MUSS** Keine Ausführung in produktiven Umgebungen ohne ausdrücklichen Auftrag; dieser Ordner beschreibt, er führt nicht aus.
- **MUSS** Änderungen an laufzeitrelevanten Dateien folgen einem Freigabepunkt (siehe [[04-Workflows/multi-agent-development]]).
- **SOLLTE** Beschreibung lässt eine zweite Person ohne Nebenwissen ausliefern.

## Zuständigkeit

[[01-Agents/devops-engineer]], Prüfung sensibler Teile durch [[01-Agents/security-reviewer]].

**Siehe auch:** [[03-Projects/_Example-Project/README]] · [[05-Tasks/backlog/DEMO-OPS-001]]
