# Apache Kafka

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Verarbeitung muss bei erneuter Zustellung sicher sein. Bei At-least-once-Verarbeitung mit fachlichen Nebenwirkungen Idempotenz über stabilen Event-/Geschäftsschlüssel und dauerhaftes Deduplizieren oder eine gleichwertige Strategie absichern. Bei Metrik-Consumern Auswirkungen von Duplikaten und zulässige Genauigkeit bewerten; eine eigene Deduplizierungstabelle ist nicht pauschal vorgeschrieben. Offsets erst entsprechend der gewählten Verarbeitungssemantik bestätigen. Fehlerpfade dürfen keine stillen Verluste verursachen. Schemaänderungen auf Kompatibilität prüfen.

Die [[02-Standards/General/cloud-native-standards|Basisregeln]] für Retry Policy, Observability und kontrollierte Beendigung einhalten. Bedingte Pflichten für Transactional Outbox, Dead Letter Topic/Queue und Competing Consumers gemäß [[02-Standards/Cloud-Design-Patterns/README]] anwenden. Gleichwertige Kopplung von Datenbankänderung und Event-Veröffentlichung nachweisen; andernfalls bei erforderlicher zuverlässiger Kopplung eine Outbox einsetzen.

## Empfehlungen (SOLLTE)
Bei erlaubten Wiederholungen transienter Fehler Backoff mit Jitter nutzen. Consumer Lag, Wiederholungen und Fehler zählen. Externe Seiteneffekte zusätzlich mit einer passenden Inbox- oder Idempotency-Key-Strategie absichern.

## Projektentscheidungen
Ordering-Key, Consumer Groups, Schemaformat, Retry-Budget, Dead-Letter-Verantwortung und Replay bestimmen. Keine pauschale Exactly-once-Zusage für Datenbank und Broker. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
