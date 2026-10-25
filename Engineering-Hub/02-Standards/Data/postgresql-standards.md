# PostgreSQL

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Schemaänderungen versioniert und nachvollziehbar migrieren. Parametrisierte Queries, passende Constraints und explizite Transaktionsgrenzen verwenden. Destruktive Migrationen vor Ausführung freigeben lassen.

## Empfehlungen (SOLLTE)
Expand/Contract für kompatible Rollouts; Indizes anhand von Zugriffsmustern und Abfrageplänen auswählen. Pooling, Timeouts und Backup-Wiederherstellung prüfen.

## Projektentscheidungen
Migrationstool, Isolation, Extensions, Aufbewahrung und Recovery-Ziele projektbezogen festlegen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
