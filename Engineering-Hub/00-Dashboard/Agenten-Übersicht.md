---
type: dashboard
tags: [agents, uebersicht]
---

# Agenten-Übersicht

Alle acht Rollen sind eigenständig lesbare Markdown-Vorlagen unter `01-Agents/`. Sie sind **Konfigurationsbeschreibungen, keine ausführbaren Agenten** – eine Rolle wird in einer Codex-Sitzung als Perspektive gelesen, nicht gestartet.

## Rollentabelle

| Rolle | Kurzprofil | Hauptverantwortung | Kernstandards |
| --- | --- | --- | --- |
| [[01-Agents/requirements-analyst\|Anforderungsanalyst]] | klärt Ziele, Geschäftsregeln und fachlichen Bedarf | Anforderungen, Akzeptanzkriterien und offene Fragen | [[07-Templates/feature-template]], [[02-Standards/Testing/testing-standards]] |
| [[01-Agents/orchestrator\|Orchestrator]] | zerlegt Aufgaben, verwaltet Abhängigkeiten und Status | Koordination und Zusammenführung | [[02-Standards/General/architecture-principles]], [[04-Workflows/agent-handover]] |
| [[01-Agents/software-architect\|Software Architect]] | entwirft Architektur, bewertet Alternativen | ADRs, arc42, Komponentengrenzen | [[02-Standards/General/architecture-principles]], [[07-Templates/adr-template]] |
| [[01-Agents/backend-developer\|Backend Developer]] | Java 21, Spring Boot 3.x, Kafka, PostgreSQL | Backend-Implementierung nach Vorgabe | `02-Standards/Java/`, [[02-Standards/Messaging/kafka-standards]], [[02-Standards/API/rest-standards]], [[02-Standards/Data/postgresql-standards]] |
| [[01-Agents/frontend-developer\|Frontend Developer]] | Angular, TypeScript | Oberflächen und Client-Logik | [[02-Standards/Frontend/angular-typescript-standards]], [[02-Standards/API/rest-standards]] |
| [[01-Agents/qa-engineer\|QA Engineer]] | JUnit 5, Mockito, Cucumber, Integrationstests | Teststrategie und Testimplementierung | [[02-Standards/Testing/testing-standards]] |
| [[01-Agents/devops-engineer\|DevOps Engineer]] | Docker, Kubernetes, Helm, CI/CD | Deployment, Monitoring, Logging | [[02-Standards/Infrastructure/docker-standards]], [[02-Standards/Infrastructure/kubernetes-standards]] |
| [[01-Agents/security-reviewer\|Security Reviewer]] | Authentifizierung, Autorisierung, Secrets, Dependencies | Sicherheitsprüfung vor Freigabe | [[02-Standards/General/security-principles]] |

## So wird eine Rolle genutzt

**Einzelmodus (Standard):** In der Codex-Aufgabe schreiben, welche Perspektive gilt – z. B. „Arbeite als Backend Developer gemäß `01-Agents/backend-developer.md`.“ Zusätzlich werden nur die nötigen Standards geladen. Es entsteht ein einzelner Agent.

**Teammodus (optional):** Nur bei explizitem Auftrag, z. B. „Aktiviere den Multi-Agenten-Workflow.“ Ablauf, Abhängigkeiten und Freigabepunkte: [[04-Workflows/multi-agent-development]]. Rollenwechsel: [[04-Workflows/agent-handover]].

## Regeln, die für alle Rollen gelten

- Eine Rolle erweitert keine Laufzeitberechtigungen. Was nicht erlaubt ist, bleibt es auch in der Teamrolle.
- Rollen verändern keine Architekturentscheidungen; sie arbeiten mit akzeptierten ADRs.
- Rollen starten keine weiteren Agenten.
- Fehlende Eingaben werden offen gemeldet, nicht still ergänzt.
- Demonstrationsdaten (`_Example-Project`, `DEMO-*`) sind niemals reale Anforderungen.

## Skills je nach Aufgabe nutzen

Rollen beschreiben Verantwortung; [[10-Skills/skill-catalog|Skills]] liefern wiederverwendbare Methoden. Anforderungsanalysten können den Skill zur Anforderungsanalyse nutzen, Entwickler und Prüfer den Review-Skill. Beide arbeiten innerhalb des bestehenden Einzelmodus. Technische Nutzung: [[10-Skills/agent-skills]].

## Neue Rolle hinzufügen

[[07-Templates/agent-template]] kopieren, elf Abschnitte ausfüllen, hier in die Tabelle aufnehmen und – falls sie Teil eines Teamablaufs wird – in [[04-Workflows/multi-agent-development]] einordnen. Siehe auch `README.md`, Abschnitt „Wie füge ich einen weiteren Agenten hinzu?“.
