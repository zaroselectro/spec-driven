---
type: project
status: demo
tags: [projekt, beispiel, demo]
---

# _Example-Project (Demonstrationsprojekt)

> [!warning] Alles auf dieser Seite ist erfunden
> Dieser Ordner zeigt, **wie** ein Projekt im Vault aussieht. Er enthält bewusst keine realen Anforderungen, keine echten Architekturentscheidungen und keine übernehmbaren Vorgaben. Vor der Nutzung in einem echten Projekt werden diese Inhalte ersetzt.

## Zweck

Veranschaulichung der Projektstruktur für Rollen, Workflows und Aufgaben.

## Stack (als Beispiel genannt)

Java 21, Spring Boot 3.x, Maven, REST, Apache Kafka, PostgreSQL, Angular/TypeScript, Docker, Kubernetes, JUnit 5, Mockito, Cucumber.

Diese Aufzählung ist ein **Beispiel**, keine Vorgabe für andere Projekte.

## Geltungsbereich

- Zentrale Standards aus [[02-Standards/README]] gelten, soweit sie hier nicht durch dokumentierte ADR ersetzt werden.
- Projektspezifische Regeln stehen in der `AGENTS.md` des Codex-Projekts und im README – nicht hier „nebenbei“.

## Aufbau dieses Ordners

| Ordner | Inhalt | Aktueller Stand |
| --- | --- | --- |
| `Requirements/` | Anforderungen und Akzeptanzkriterien | Platzhalter – bewusst für spätere Inhalte |
| `Architecture/arc42/` | laufende Architekturbeschreibung | Platzhalter mit Gliederungsvorschlag |
| `Architecture/ADR/` | Architektur-Entscheidungen | Platzhalter mit Registrierungs-Hinweis |
| `API/` | Schnittstellenverträge | Platzhalter |
| `Tasks/` | projektspezifische Aufgaben | Platzhalter (gleiche Felder wie [[05-Tasks/README]]) |
| `Testing/` | Teststrategie und Testbefehle | Platzhalter |
| `Operations/` | Auslieferung und Betrieb | Platzhalter |

## Verknüpfte Beispielaufgaben (Demonstrationsdaten)

- [[05-Tasks/backlog/DEMO-BE-001]] (Backend)
- [[05-Tasks/backlog/DEMO-ARCH-001]] (Architektur)
- [[05-Tasks/backlog/DEMO-OPS-001]] (DevOps)

## Nächster Schritt für ein echtes Projekt

1. Ordner kopieren/ableiten und diesen README durch [[07-Templates/project-template]] ersetzen.
2. Anforderungen unter `Requirements/` eintragen (nur bestätigte).
3. `AGENTS.md` im Codex-Projekt anlegen: [[08-Codex/project-integration]].
4. Erste ADRs nur bei echten Entscheidungen anlegen: [[07-Templates/adr-template]].
