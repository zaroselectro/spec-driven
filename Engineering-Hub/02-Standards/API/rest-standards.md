# REST

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
HTTP-Methoden und Statuscodes semantisch verwenden. Authentifizierung und objektbezogene Autorisierung serverseitig prüfen. Fehler konsistent ohne Stacktraces an Clients liefern. Verträge auf Rückwärtskompatibilität prüfen.

Für ausgehende API-Aufrufe Timeout und Retry Policy gemäß [[02-Standards/General/cloud-native-standards]] festlegen. Bedingte Pflichten aus [[02-Standards/Cloud-Design-Patterns/README]] anwenden, insbesondere Circuit Breaker, Bulkhead und Rate Limiting bei den dort benannten Risiken.

## Empfehlungen (SOLLTE)
OpenAPI-Vertrag, Pagination für unbeschränkte Listen und stabile Fehlercodes verwenden.

## Projektentscheidungen
Versionierung, Fehlerformat, Idempotency-Key-Semantik und Deprecation-Fristen festlegen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
