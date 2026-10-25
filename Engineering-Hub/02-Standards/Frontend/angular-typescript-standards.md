# Angular und TypeScript

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
TypeScript-Typen konsequent verwenden; externe Daten validieren. Secrets gehören nicht ins Frontend. Lade-, Leer- und Fehlerzustände behandeln; serverseitige Autorisierung nicht durch UI-Prüfungen ersetzen.

## Empfehlungen (SOLLTE)
Kohärente Komponenten, zugängliche Semantik, Tastaturbedienung und getrennte Datenzugriffe bevorzugen. Subscription-Lebenszyklen beachten.

## Projektentscheidungen
Angular-/TypeScript-Versionen, Zustandsverwaltung, Designsystem und Browserunterstützung bestimmen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
