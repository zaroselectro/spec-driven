# Importe und externe Identitäten

## Geltungsbereich

Gilt bei Auswahl für wiederholte Importe mit externen Identitäten oder Gruppenzuordnungen. Die folgenden Regeln gelten für die jeweils benötigten Anwendungsfälle; physische Löschung und Aufbewahrung sind Projektentscheidungen.

## Wiederholte Importe und externe Identitäten

- Interne Primärschlüssel und externe Anbieterkennungen getrennt modellieren; externe Identitäten durch geeignete eindeutige Datenbankbedingungen absichern.
- Wiederholte Importe aktualisieren vorhandene Datensätze ohne Duplikate. Mehrfachzuordnungen durch eindeutige Paare absichern.
- Bestehende Zuordnungen erst nach einer erfolgreichen vollständigen Abfrage abgleichen. Fehler oder unvollständige Ergebnisse dürfen nicht als leere Ergebnismenge aktive Zuordnungen löschen.
- Entfernte Konfigurationsgruppen heben ihre aktiven Zuordnungen auf; physische Datenlöschung ist eine gesonderte fachliche Entscheidung.
- Integrationstests für idempotente Importe, eindeutige externe IDs, überlappende Gruppen und das Entfernen einzelner Zuordnungen ergänzen, sofern diese Anwendungsfälle benötigt werden.

