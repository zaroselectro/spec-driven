---
id: DEMO-OPS-001
title: "Auslieferungskonfiguration für die Beispielanwendung vorbereiten"
project: _Example-Project
type: devops
mode: single-agent
status: backlog
priority: mittel
assigned_to: devops-engineer
review_by: security-reviewer
depends_on: [DEMO-BE-001]
created: 2026-10-20
updated: 2026-10-20
tags: [demo]
---

> [!warning] Demonstrationsdaten
> Erfundene Beispielaufgabe zur Veranschaulichung des Task-Formats. Keine reale Anforderung.

# DEMO-OPS-001 – Auslieferungskonfiguration vorbereiten

## Ziel

Die Beispielanwendung wird als reproduzierbares, überprüfbares Auslieferungspaket beschrieben: Image, Laufzeitkonfiguration und Manifeste entstehen als Dokumentation in `03-Projects/_Example-Project/Operations/`.

## Akzeptanzkriterien

1. Container-Beschreibung nach [[02-Standards/Infrastructure/docker-standards]]: Multi-Stage, feste Basisbildversion, Nicht-Root-Benutzer, keine Secrets im Image.
2. Laufzeitmanifest nach [[02-Standards/Infrastructure/kubernetes-standards]]: Requests/Limits, Liveness-/Readiness-Prüfungen, SecurityContext, dokumentierte Konfigurationsvariablen.
3. Konfigurationsmatrix: Variable, Zweck, Quelle, Standard, ob geheim.
4. Beschriebener Rollback-Weg und beschriebene Reihenfolge des Auslieferungsschritts.
5. Kein Deployment wird ohne ausdrücklichen Auftrag ausgeführt – Aufgabe endet mit vorbereitetem Paket.

## Umfang und Grenzen

- **Im Umfang:** Konfigurations- und Manifestdateien samt Dokumentation.
- **Nicht im Umfang:** Ausführung in einer produktiven Umgebung, Festlegung von Cluster-Anbieter, Clustereinrichtung.

## Voraussetzungen

- `DEMO-BE-001` (damit Konfigurationsbedarfe der Anwendung bekannt sind).
- Falls abweichendes Paket nötig wird: zuerst [[04-Workflows/architecture-design]].

## Zu ziehende Regeln

- [[02-Standards/Infrastructure/docker-standards]] · [[02-Standards/Infrastructure/kubernetes-standards]]
- [[02-Standards/General/security-principles]] (Secrets, Rechte)

## Review

`review_by: security-reviewer` – prüft insbesondere Secrets-Quellen, Benutzerrechte und Konfigurationsangaben. Auslieferung in eine produktive Umgebung ist ein separater Freigabepunkt (siehe [[04-Workflows/multi-agent-development]]).
