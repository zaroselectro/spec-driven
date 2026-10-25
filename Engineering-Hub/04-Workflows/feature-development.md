---
type: workflow
tags: [workflow, feature, umsetzung]
---

# Workflow: Feature-Entwicklung

Ablauf für eine zusammenhängende funktionale Änderung – vom Auftrag bis zur Übergabe. Im **Single-Agent-Modus** als schlanke Kette durchlaufen; im Teammodus mit Rollen besetzen (siehe Spalte rechts).

## Ablauf

### 1. Auftrag klären
- Was soll fachlich erreicht werden? Welche Akzeptanzkriterien sind prüfbar?
- Wo liegt die Aufgabe? Größere Vorhaben als Task anlegen: [[07-Templates/task-template]], Ablage `05-Tasks/` oder `03-Projects/<projekt>/Tasks/`.
- Verbindliche Regeln sichten: Projekt-`AGENTS.md`, betroffene Standards, akzeptierte ADRs.
- Bei Projekten mit Specs-Basis (`specs/`) zusätzlich: **vor der betroffenen Implementierung** die offenen Projektentscheidungen der aktiven Erweiterungen klären (z. B. Routen und Fehlerformat bei `http-api`, Produktivdatenbank bei `persistence-jpa`) – Fehlendes wird benannt und gelöst, nicht stillschweigend angenommen; unabhängige Arbeiten laufen weiter.
- *Teamrolle:* [[01-Agents/orchestrator]]

### 2. Architektur prüfen (nur wenn nötig)
- Passt die Änderung in die bestehende Struktur? Wenn ja: umsetzen, kein ADR.
- Wenn nein (neue Komponente, neuer Technologieeinsatz, Bruch mit Vertrag): [[04-Workflows/architecture-design]] durchlaufen, **vor** der Implementierung.
- *Teamrolle:* [[01-Agents/software-architect]]

### 3. Implementieren
- Schichten beachten: API / Geschäftslogik / Persistenz getrennt (siehe [[02-Standards/General/architecture-principles]]).
- Fachstandards der betroffenen Technologie ziehen (Java, Spring Boot, REST, Kafka, PostgreSQL, Angular …).
- Fehlerbehandlung und strukturierte Protokollierung an den betroffenen Stellen mitdenken; keine Secrets im Code.
- Konfigurationsbedarfe notieren statt selbst Werte zu setzen.
- *Teamrolle:* [[01-Agents/backend-developer]] bzw. [[01-Agents/frontend-developer]]

### 4. Tests ergänzen
- Akzeptanzkriterien → Testfälle; Fehlerpfade und Idempotenz beachten.
- Projekt-Befehle ausführen und Ergebnis nennen.
- *Teamrolle:* [[01-Agents/qa-engineer]]

### 5. Prüfen lassen
- Änderung nach [[04-Workflows/code-review]] bewerten.
- Sicherheitsrelevante Teile durch [[01-Agents/security-reviewer]]; Konfigurations-/Auslieferungsteile durch [[01-Agents/devops-engineer]].
- Befunde nach Schwere behandeln: kritisch/hoch blockieren.

### 6. Abschluss
- Aufgabenstatus aktualisieren (`review` → `done`), Übergabe dokumentieren: [[04-Workflows/agent-handover]].
- Kurzbericht: Umfang, Prüfergebnisse, offene Punkte, ggf. Folgeaufgaben.

## Abhängigkeiten und Parallelität

- Backend- und Frontendarbeit können parallel laufen, **sobald der Schnittstellenvertrag steht**; vorher Parallelität schafft nur Doppelarbeit.
- QA startet, sobald ein prüfbarer Stand existiert; Security-Prüfung bei Sicherheitsrelevanz unabhängig davon.
- DevOps-Vorbereitung startet, wenn Konfigurationsbedarfe bekannt sind, und endet vor dem Freigabepunkt.

## Was vermieden wird

- Spekulation über Anforderungen, die niemand gestellt hat.
- Refactoring ohne Auftrag (Ausnahme: Stellen, die die Aufgabe zwingend berührt).
- Feature ohne Fehlerpfade und ohne Tests.
- Deploymentschritte ohne Freigabe (siehe [[04-Workflows/multi-agent-development]]).

**Siehe auch:** [[04-Workflows/single-agent-development]] · [[04-Workflows/code-review]] · [[07-Templates/feature-template]] · [[05-Tasks/README]]
