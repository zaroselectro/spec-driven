# REST

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
HTTP-Methoden und Statuscodes semantisch verwenden. Authentifizierung und objektbezogene Autorisierung serverseitig prüfen. Fehler konsistent ohne Stacktraces an Clients liefern. Verträge auf Rückwärtskompatibilität prüfen.

## Empfehlungen (SOLLTE)
OpenAPI-Vertrag, Pagination für unbeschränkte Listen, Timeouts und stabile Fehlercodes verwenden. Wiederholungen nur für passende idempotente Operationen.

## Projektentscheidungen
Versionierung, Fehlerformat, Idempotency-Key-Semantik und Deprecation-Fristen festlegen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
