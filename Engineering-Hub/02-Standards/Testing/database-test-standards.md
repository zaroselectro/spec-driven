# Datenbankintegrationstests

## Geltungsbereich

Gilt bei Auswahl für Datenbankintegrationstests. Die folgenden Regeln sind verbindlich; Datenbanksystem, Version und Teststrategie werden im Projekt festgelegt.

- Migrationen und reale Datenzugriffe einschließlich Datenmapping gegen eine isolierte Testdatenbank prüfen. Produktive oder gemeinsam genutzte Datenbanken sind ausgeschlossen.
- Testdaten durch Rollbacks oder gezieltes Aufräumen isolieren.
- H2 bildet herstellerspezifisches Verhalten anderer Datenbanken nicht vollständig ab. Grenzen dokumentieren; H2 belegt keine vollständige Produktionskompatibilität.
- Für PostgreSQL sind produktionsnahe Tests mit Testcontainers empfohlen. Bei Testcontainers eine zur produktiven Hauptversion passende Version festlegen.
- H2 ersetzt keine ausdrücklich gewählten Tests gegen das produktive Datenbanksystem. Eine verfügbare Containerlaufzeit ist Voraussetzung von Testcontainers; fehlende Voraussetzungen als nicht ausgeführte Prüfung melden, niemals unbemerkt überspringen oder als Erfolg ausgeben.
