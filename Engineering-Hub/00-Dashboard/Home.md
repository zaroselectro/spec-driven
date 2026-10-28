---
type: dashboard
tags: [dashboard, einstieg]
---

# Home – Engineering Vault

Startseite der Wissensbasis für Agentic Coding. Alle Links sind interne Obsidian-Verweise.

## Einstieg in 60 Sekunden

1. Vault als Ordner in Obsidian öffnen (Anleitung in `README.md`, Abschnitt „Wie öffne ich sie in Obsidian?“).
2. Für eine Aufgabe: **Single-Agent-Workflow** lesen → [[04-Workflows/single-agent-development]].
3. Nur die einschlägigen Standards laden (siehe Tabelle unten), nicht den ganzen Vault.
4. Aufgabe in `05-Tasks/` anlegen, wenn sie größer als eine Kleinigkeit ist → [[05-Tasks/README]].

> [!note] Rollen sind Vorlagen
> Die Dateien unter `01-Agents/` beschreiben Konfigurationsrollen. Sie starten keine Agenten. Der Multi-Agent-Modus wird nie automatisch aktiviert.

## Bereiche

### Agenten
- [[00-Dashboard/Agenten-Übersicht]] – wer macht was
- [[01-Agents/requirements-analyst]] – Anforderungen, Geschäftsregeln, Akzeptanzkriterien
- [[01-Agents/orchestrator]] – Koordination, Delegation, Status
- [[01-Agents/software-architect]] – Architektur, ADRs, arc42
- [[01-Agents/backend-developer]] – Java, Spring Boot, Kafka, PostgreSQL
- [[01-Agents/frontend-developer]] – Angular, TypeScript
- [[01-Agents/qa-engineer]] – JUnit 5, Mockito, Cucumber, Integrationstests
- [[01-Agents/devops-engineer]] – Docker, Kubernetes, Helm, CI/CD
- [[01-Agents/security-reviewer]] – Authentifizierung, Secrets, Abhängigkeiten

### Standards
- [[02-Standards/README]] – Ordnung und Pflege der Standards
- Allgemein: [[02-Standards/General/architecture-principles]] · [[02-Standards/General/clean-code]] · [[02-Standards/General/security-principles]]
- Java & Spring: [[02-Standards/Java/java-standards]] · [[02-Standards/Java/spring-boot-standards]] · [[02-Standards/Java/maven-standards]]
- Schnittstellen & Daten: [[02-Standards/API/rest-standards]] · [[02-Standards/Data/postgresql-standards]] · [[02-Standards/Messaging/kafka-standards]]
- Frontend: [[02-Standards/Frontend/angular-typescript-standards]]
- Qualität & Betrieb: [[02-Standards/Testing/testing-standards]] · [[02-Standards/Infrastructure/docker-standards]] · [[02-Standards/Infrastructure/kubernetes-standards]]

### Projekte
- [[03-Projects/README]] – Anlegen und Pflege von Projekten
- [[03-Projects/_Example-Project/README]] – Demonstrationsprojekt (Beispieldaten!)
- Specs-Basis `specs/` (außerhalb des Vaults, Pfadangabe statt Wikilink) – Spring-Boot-Vorlage für Projekte, Geltungsreihenfolge siehe [[02-Standards/README]], Übernahme siehe [[08-Codex/project-integration]]

### Aufgaben
- [[05-Tasks/README]] – Statusablauf und Felder
- Beispiele: [[05-Tasks/backlog/DEMO-BE-001]] · [[05-Tasks/backlog/DEMO-ARCH-001]] · [[05-Tasks/backlog/DEMO-OPS-001]]

### Workflows
- [[04-Workflows/single-agent-development]] – **Standardmodus**
- [[04-Workflows/multi-agent-development]] – optional, nur auf expliziten Auftrag
- [[04-Workflows/architecture-design]] · [[04-Workflows/feature-development]] · [[04-Workflows/code-review]] · [[04-Workflows/agent-handover]]

### Templates
- [[07-Templates/README]] – Übersicht aller Vorlagen
- [[07-Templates/project-template]] · [[07-Templates/feature-template]] · [[07-Templates/adr-template]] · [[07-Templates/task-template]] · [[07-Templates/agent-template]] · [[07-Templates/review-template]]

### Codex-Integration
- [[08-Codex/codex-integration]] – Konzept und Grenzen
- [[08-Codex/project-integration]] – bestehendes Projekt anbinden
- [[08-Codex/prompt-examples]] – fertige Prompt-Beispiele
- [[08-Codex/AGENTS]] – Regeln dieses Vault-Bereichs

### Wissen & Pflege
- [[06-Knowledge/README]] – Hintergrundwissen (unverbindlich)
- [[09-Maintenance/creation-report]] – Bestands- und Prüfbericht

## Häufige Aufgaben

| Ich will … | dann … |
| --- | --- |
| eine kleine Änderung umsetzen | [[04-Workflows/single-agent-development]] durchlaufen |
| ein neues Projekt anlegen | [[07-Templates/project-template]] nutzen, Gliederung aus [[03-Projects/README]]; für Spring-Boot optional Specs-Basis übernehmen |
| eine Regel ändern | zuständige Datei in `02-Standards/` bearbeiten oder ADR im Projekt anlegen |
| eine Aufgabe verteilen | [[07-Templates/task-template]] nutzen, Status in `05-Tasks/` pflegen |
| mehrere Rollen koordinieren | [[04-Workflows/multi-agent-development]] **explizit aktivieren** |
| eine Aufgabe übergeben | [[04-Workflows/agent-handover]] mit [[07-Templates/review-template]] |

## Konsolidierter Bestand
[[specs/README|Optionale Spring-Boot-Specs]] · [[09-Maintenance/rule-sources|Regelquellen]] · [[09-Maintenance/consolidation-report|Zusammenführungsbericht]]

## Neues Projekt starten
[[03-Projects/Examples/agentic-project|Agentisch anlegen]] · [[03-Projects/Examples/selected-specs-project|Ohne agentischen Ablauf mit ausgewählten Specs]]
