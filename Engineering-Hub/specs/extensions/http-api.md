# Erweiterung: HTTP-API

## Zweck und Geltungsbereich

ID: `http-api`. Gilt ausschließlich bei ausdrücklicher Aktivierung für eingehende HTTP-APIs. Aktiviert weder Persistenz noch Security.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen.

## Offene Projektentscheidungen

In `specs/project.md` festlegen: angebotene Operationen und Routen, Request- und Response-DTOs, Validierung, Statuscodes und Fehlerformat sowie gegebenenfalls API-Versionierung. Benötigte Authentifizierung und Autorisierung müssen durch eine aktive Erweiterung oder ausdrückliche Projektregeln festgelegt werden.

## Technische Regeln

- Ergänze nur die für die gewählte HTTP-Implementierung und Validierung benötigten Dependencies. Versionsverwaltung erfolgt gemäß Basis.
- Zugangsdaten und umgebungsabhängige Konfiguration werden extern zugeführt.

## Architektur

- Controller und Eingangsvalidierung liegen unter `<fachmodul>.api`; Anwendungsfälle verbleiben in `services`.
- Es gelten die DTO- und Abhängigkeitsgrenzen der Basis. Controller rufen Services auf und greifen nicht direkt auf ausgehende Anbindungen zu.
- Übersetze dokumentierte Fehler zentral in die projektspezifischen HTTP-Antworten. Fachliche Fehlerverträge bleiben unabhängig von HTTP.

## Tests

- Die Unit-Test- und Maven-Regeln der Basis gelten zusätzlich.
- Prüfe mindestens einen erfolgreichen Ablauf und einen relevanten Validierungs- oder Fehlerfall über die HTTP-Verarbeitung mit tatsächlichem Controller, tatsächlichem Service und gegebenenfalls zentraler Fehlerbehandlung.
- Binde benötigte Validierung und Serialisierung ein; ausgehende Anbindungen dürfen an den Ports gemockt werden. Eine Datenbank wird dafür nicht vorausgesetzt.

## Abnahmekriterien

- Operationen, Validierung und Fehlerantworten entsprechen den dokumentierten Projektentscheidungen.
- Die verpflichtenden API-Integrationstests laufen im Testablauf der Basis erfolgreich.
- HTTP-Details bleiben in der API-Schicht; fachliche Port-Verträge bleiben davon unabhängig.

## Abweichungen von der Basis

Keine.
