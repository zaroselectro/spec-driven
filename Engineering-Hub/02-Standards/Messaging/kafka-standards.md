# Apache Kafka

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Verarbeitung muss bei erneuter Zustellung sicher sein: Idempotenz über fachlichen Schlüssel und dauerhaftes Deduplizieren oder gleichwertige Strategie. Offsets erst entsprechend der gewählten Verarbeitungssemantik bestätigen. Fehlerpfade dürfen keine stillen Verluste verursachen. Schemaänderungen auf Kompatibilität prüfen.

## Empfehlungen (SOLLTE)
Transiente Fehler begrenzt mit Backoff wiederholen; permanente Fehler über kontrollierten Fehlerkanal behandeln. Consumer Lag, Wiederholungen und Fehler zählen. Externe Seiteneffekte mit Outbox/Inbox bewerten.

## Projektentscheidungen
Ordering-Key, Consumer Groups, Schemaformat, Retry-Budget, Dead-Letter-Verantwortung und Replay bestimmen. Keine pauschale Exactly-once-Zusage für Datenbank und Broker. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
