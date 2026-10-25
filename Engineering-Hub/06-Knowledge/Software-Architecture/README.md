---
type: knowledge
topic: software-architektur
tags: [wissen, architektur]
---

# Wissen: Software-Architektur

Hintergrund zu Architekturarbeit. **Verbindlich** sind [[02-Standards/General/architecture-principles]] und die Regeln um ADRs.

## Architektur ist Entscheidung mit Spur

Architektur besteht aus den Entscheidungen, die teuer sind, sie zu ändern. Alles andere darf sich im Code frei entwickeln. Drei Werkzeuge tragen das im Vault:

1. **arc42** – Gliederung für die laufende Beschreibung (`03-Projects/<projekt>/Architecture/arc42/`).
2. **ADRs** – je Entscheidung eine Datei mit Problem, Optionen, Ergebnis, Konsequenzen.
3. **Standards** – was für viele Projekte gleichermaßen gilt (`02-Standards/`).

## ADR-Methode in Kurzform

- **Problem zuerst:** Ohne klares Problem wird jede Option gut klingen.
- **Optionen ernsthaft prüfen:** mindestens zwei bei wirklicher Tragweite; Bewertungskriterien **vor** dem Vergleich festlegen.
- **Konsequenzen mitnehmen:** Jede Wahl kostet etwas (Aufwand, Flexibilität, Betrieb) – das gehört ins Dokument.
- **Status pflegen:** `vorgeschlagen` verpflichtet nicht; `akzeptiert` gilt, bis ein neues ADR es ersetzt.
- **Nichts stillschweigend ändern:** wer die Entscheidung ändert, schreibt es auf.

## Bausteine und Grenzen

- Eine Grenze (Komponente, Modul, Dienst) ist dort sinnvoll, wo **Änderungsgründe** sich unterscheiden – nicht, weil es „sauberer“ wirkt.
- Jede Grenze kostet: Kommunikation, Verzögerung, mehr Konfiguration. Granularität ist eine Abwägung, keine Tugend.
- Schnittstellen sind Verträge: was sie versprechen (Rückwärtskompatibilität, Fehlerformate), muss gehalten werden (siehe [[02-Standards/API/rest-standards]]).

## Qualitätseigenschaften im Zusammenspiel

| Eigenschaft | Wird oft verwechselt mit | Klärung |
| --- | --- | --- |
| Testbarkeit | „viele Tests“ | Struktur ermöglicht Tests ohne künstliche Konstrukte |
| Observability | „Logs vorhanden“ | Zustand ist aus Metriken/Logs/Probes erschließbar |
| Wartbarkeit | „wenig Code“ | Änderungen an einer Stelle bleiben an einer Stelle |
| Rückwärtskompatibilität | „funktioniert noch“ | Bestandsnutzer bleiben ohne Absprache funktionsfähig |

## Typische Fehlentscheidungen

- Technologiewahl **vor** Problemklärung (Lösung sucht Problem).
- Abstraktion ohne zweite konkrete Verwendung.
- Standardregeln projektspezifisch machen („bei uns gilt das nicht“) ohne ADR-Begründung.
- Dokumentation, die den Code beschreibt statt die Entscheidung.

## Einstieg in dieses Vault

Prinzipien: [[02-Standards/General/architecture-principles]] · Ablauf: [[04-Workflows/architecture-design]] · Vorlage: [[07-Templates/adr-template]] · Rolle: [[01-Agents/software-architect]]

**Siehe auch:** [[06-Knowledge/README]]
