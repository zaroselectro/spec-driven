---
id: DEMO-BE-001
title: "Kafka-Consumer um kontrollierte Fehlerbehandlung ergänzen"
project: _Example-Project
type: backend
mode: single-agent
status: backlog
priority: mittel
assigned_to: backend-developer
review_by: qa-engineer
depends_on: []
created: 2026-10-20
updated: 2026-10-20
tags: [demo]
---

> [!warning] Demonstrationsdaten
> Erfundene Beispielaufgabe zur Veranschaulichung des Task-Formats. Keine reale Anforderung.

# DEMO-BE-001 – Kafka-Consumer um kontrollierte Fehlerbehandlung ergänzen

## Ziel

Der bestehende Kafka-Consumer verarbeitet fehlerhafte Nachrichten kontrolliert statt sie still zu verwerfen oder den Verarbeitungslauf zu beenden.

## Akzeptanzkriterien

1. Vorübergehende Fehler (z. B. kurzfristige Nichtverfügbarkeit eines Aufrufs) führen zu begrenzter Wiederholung mit Pause.
2. Nach erfolglosen Wiederholungen wird die Nachricht mit Ursache und Zeitpunkt in den vereinbarten Fehlerbereich des Projekts übernommen.
3. Der Verarbeitungsfortschritt bleibt erhalten: ein Absturz nach erfolgreicher Verarbeitung führt nicht zu doppelter Wirkung.
4. Fehlerhafte Vorgänge sind über Protokoll/Metrik sichtbar.
5. Tests decken ab: erfolgreiche Verarbeitung, wiederholter vorübergehender Fehler, endgültig fehlerhafte Nachricht, erneutes Eintreffen derselben Nachricht.

## Umfang und Grenzen

- **Im Umfang:** Consumer-Verarbeitung, Fehlerpfade, zugehörige Tests.
- **Nicht im Umfang:** Topic-Umstrukturierung, Änderung des Nachrichtenschemas (erfordert eigenes Architekturverfahren), Deployment.

## Voraussetzungen

- Keine.
- Vorhandene ADRs zu Messaging beachten; Abweichungen erfordern [[04-Workflows/architecture-design]].

## Zu ziehende Regeln

- [[02-Standards/Messaging/kafka-standards]] (Fehlerbehandlung, Idempotenz, Kompatibilität)
- [[02-Standards/Testing/testing-standards]]
- Projekt-`AGENTS.md` des `_Example-Project`

## Review

`review_by: qa-engineer` – Ergebnis nach [[07-Templates/review-template]] in dieser Datei festhalten. Sicherheitsrelevanz: keine.
