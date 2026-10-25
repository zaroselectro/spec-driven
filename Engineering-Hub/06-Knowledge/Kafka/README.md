---
type: knowledge
topic: kafka
tags: [wissen, kafka, messaging]
---

# Wissen: Kafka

Hintergrund zur Nachrichtenverarbeitung mit Apache Kafka. **Verbindlich** ist ausschließlich [[02-Standards/Messaging/kafka-standards]].

## Grundmodell in Kurzform

- Ein **Topic** ist in **Partitionen** geteilt; pro Partition ist die Reihenfolge definiert.
- Ein **Key** entscheidet, welche Partition eine Nachricht bekommt – gleicher Key, gleiche Partition, damit Reihenfolge je Objekt erhalten bleibt.
- **Konsumenten** lesen in Gruppen; die Aufteilung der Partitionen erfolgt innerhalb der Gruppe. Ein Neustart verteilt die Partitionen neu.
- **Offsets** markieren, bis wohin gelesen wurde; sie sind die Grundlage dafür, dass Arbeit nach einem Absturz weitergeht.

## Liefergarantien – was wirklich passiert

| Garantie | Bedeutung | Konsequenz |
| --- | --- | --- |
| mindestens einmal | Nachlieferung möglich | Konsument muss **idempotent** sein |
| genau einmal (End-to-End) | keine Doppelwirkung, verteilt erreichbar | höherer Aufwand, Anforderungen an beide Seiten |
| effektiv genau einmal | Doppelwirkung unwahrscheinlich gemacht | Kompromiss: Deduplizierung + idempotente Verarbeitung |

Die Wahl ist pro Vorgang zu treffen und zu dokumentieren – nicht pro Projekt pauschal.

## Warum Idempotenz Pflicht ist

Weil Nachlieferung passiert: bei Absturz nach Verarbeitung vor Speicherung des Freigabezeichens, bei Wiederaufnahme, bei Neuaufteilung. Dieselbe Nachricht mehrfach zu verarbeiten darf deshalb keinen doppelten Effekt haben – typische Mittel: Ereignis-ID als Unique-Konstraint, idempotente Schreiboperation, Deduplizierungsfenster.

## Fehlerfälle im Alltag

1. **Vorübergehend** (Timeout, kurz nicht erreichbar): begrenzte Wiederholung mit Pause.
2. **Dauerhaft** (Nachricht selbst unverarbeitbar): in den Fehlerbereich mit Ursache, sichtbar machen, später gezielt aufarbeiten.
3. **Dauerhaft blockierend** (Endlosschleife durch fehlerhaften Key/Logik): erst stoppen, dann beheben – sonst wächst der Versatz unbemerkt.

Details und Regeln: [[02-Standards/Messaging/kafka-standards]] (Abschnitt Fehlerbehandlung).

## Betrieb

Wichtig sind Versatz (Lag), Verarbeitungsdauer und Fehlerquote – ohne diese Kennzahlen ist Überlastung unsichtbar. Sicherheits- und Zugangseinstellungen stammen aus der Umgebung, nie aus dem Repository.

## Einstieg in dieses Vault

Standard: [[02-Standards/Messaging/kafka-standards]] · Persistenzfolge: [[02-Standards/Data/postgresql-standards]] · Rolle: [[01-Agents/backend-developer]]

**Siehe auch:** [[06-Knowledge/README]]
