---
id: DEMO-ARCH-001
title: "ADR: Umgang mit gleichzeitigen Statusänderungen derselben Aufgabe"
project: _Example-Project
type: architecture
mode: single-agent
status: backlog
priority: niedrig
assigned_to: software-architect
review_by: orchestrator
depends_on: []
created: 2026-10-20
updated: 2026-10-20
tags: [demo]
---

> [!warning] Demonstrationsdaten
> Erfundene Beispielaufgabe zur Veranschaulichung des Task-Formats. Keine reale Anforderung.

# DEMO-ARCH-001 – ADR zu gleichzeitigen Statusänderungen

## Ziel

Entscheiden, wie das Projekt mit konkurrierenden Statusänderungen derselben Aufgabe umgeht (zwei Akteure ändern denselben Zustand nahezu gleichzeitig).

## Aufgabe

1. Ausgangslage und Problem beschreiben (inkl. konkreter Nutzungsfall aus dem Beispielprojekt).
2. Mindestens zwei Alternativen bewerten, z. B.:
   - Optimistische Sperre mit Versionskennung,
   - Pessimistische Sperre bzw. serialisierende Prüfung.
3. Bewertungskriterien vor dem Vergleich festlegen (Datenintegrität, Aufwand, Verhalten unter Last).
4. Entwurf nach [[07-Templates/adr-template]] erstellen, Status `vorgeschlagen`.
5. [[04-Workflows/architecture-design]] durchlaufen: Feedback aus Implementierung und Security einholen, dann Freigabe beim Auftraggeber.

## Akzeptanzkriterien

- ADR-Datei unter `03-Projects/_Example-Project/Architecture/ADR/` mit allen Pflichtabschnitten.
- Entscheidung ist mit Konsequenzen und Rückkehrpfad dokumentiert.
- Betroffene Standards geprüft; Abweichung von **MUSS**-Regeln – falls vorhanden – ausdrücklich begründet.
- Status in dieser Aufgabe nach Freigabe auf `done` setzen, `updated` aktualisieren.

## Voraussetzungen

- Keine. Bestehende ADRs des Projekts werden vorher gelesen.

## Zu ziehende Regeln

- [[02-Standards/General/architecture-principles]] · [[02-Standards/Data/postgresql-standards]]

## Review

`review_by: orchestrator` – der Orchestrator prüft Vollständigkeit und Konsistenz, erteilt aber **keine** fachliche Architekturfreigabe; die liegt beim Auftraggeber.
