# Erweiterung: JPA-Persistenz

## Zweck und Geltungsbereich

ID: `persistence-jpa`. Gilt ausschließlich bei ausdrücklicher Aktivierung für relationale Persistenz mit Spring Data JPA und Liquibase. Aktiviert weder HTTP noch andere Erweiterungen.

## Voraussetzungen

- Basis-Specs; keine weiteren Erweiterungen.
- Eine fachliche Anforderung an relationale Persistenz.

## Offene Projektentscheidungen

In `specs/project.md` festlegen: produktives Datenbanksystem, Datenbankanbindungen, persistierte Fachbereiche, Konfigurationsquellen und notwendige herstellerspezifische Abweichungen sowie Testdatenbankstrategie (H2 oder produktionsnahes Datenbanksystem mit Testcontainers). Für PostgreSQL wird Testcontainers empfohlen. Zugangsdaten werden extern zugeführt, nicht in Specs hinterlegt.

## Technische Regeln

Eine Datenbank wird nur eingeführt, wenn sie fachlich benötigt wird. Schemaänderungen erfolgen über Liquibase; für den Datenzugriff wird möglichst Spring Data verwendet. Für relationale Datenbanken gilt:

- Ergänze die benötigten JPA-, Liquibase- und Datenbanktreiber-Dependencies sowie die Dependencies der gewählten Testdatenbankstrategie. Versionsverwaltung erfolgt gemäß Basis.
- Verwende möglichst Spring Data JPA. Begründe projektspezifische Ausnahmen.
- Entitäten liegen unter `adapter.<datenbankservice>.persistence.<fachbereich>.entity`, Repositories unter `adapter.<datenbankservice>.persistence.<fachbereich>.repository`. Damit bleibt die Persistenz innerhalb des Adapters fachlich gegliedert. Im fachlichen Modul befinden sich nur der Datenbankport und die benötigten DTOs, keine Entitäten, Repositories oder Persistenzimplementierungen.
- Der Primärschlüssel heißt ohne abweichende Vorgabe `idx` und hat den Java-Typ `java.util.UUID`. Bei JPA verwende `@Id` und beispielsweise `@GeneratedValue(strategy = GenerationType.UUID)`. Definiere auch in Liquibase eine passende UUID-Spalte mit Primärschlüsselbedingung.
- Vermeide Lomboks `@Data` auf JPA-Entitäten, da generierte Gleichheits- und String-Methoden problematische Beziehungen oder veränderliche Felder erfassen können. Wähle Getter, Setter, Konstruktoren und gegebenenfalls Gleichheitsmethoden bewusst.
- Liquibase ist die einzige Quelle für Schemaänderungen. Verwende einen zentralen Einstieg unter `src/main/resources/db/changelog/db.changelog-master.yaml` und fachlich gegliederte, eingebundene Changesets.
- Verändere bereits angewendete Changesets nicht nachträglich; ergänze neue Changesets.
- Verwende keine parallele Schemaerzeugung durch Hibernate oder `schema.sql`. Bei JPA setze `spring.jpa.hibernate.ddl-auto=validate` und `spring.jpa.open-in-view=false`.

## Architektur

Die allgemeinen Port-, DTO- und Transaktionsregeln der Basis gelten. Die Persistenzstruktur ergänzt das Beispiel der Basis nur bei Bedarf:

```text
<basispaket>
├── order/port/OrderDatabasePort.java
└── adapter/orderdatabase/
    ├── OrderDatabaseAdapter.java
    ├── configuration/
    └── persistence/order/
        ├── entity/
        ├── repository/
        └── converter/
```

## Eingebundener Standard

Bei Aktivierung gelten zusätzlich die Regeln aus [import-standards](../../02-Standards/Data/import-standards.md) für den berührten Bereich. Die Datei muss beim Übernehmen mitgeführt werden; die Kopierzuordnung steht in der README der Sammlung. Dies aktiviert keine weitere Erweiterung. Projektentscheidungen und begründete Abweichungen stehen in `specs/project.md`.

## Eingebundener Standard

Bei Aktivierung gelten zusätzlich die Regeln aus [database-test-standards](../../02-Standards/Testing/database-test-standards.md) für den berührten Bereich. Die Datei muss beim Übernehmen mitgeführt werden; die Kopierzuordnung steht in der README der Sammlung. Dies aktiviert keine weitere Erweiterung. Projektentscheidungen und begründete Abweichungen stehen in `specs/project.md`.

## Tests

- Testdaten und Testkonfiguration liegen unter `src/test/resources`. Isoliere Testdaten durch Rollbacks oder gezieltes Aufräumen.
- Die gewählte Testdatenbankstrategie umsetzen und die regulären Liquibase-Migrationen ausführen. Die Unit-Test- und Maven-Regeln der Basis gelten zusätzlich.
- Prüfe für jede Datenbankanbindung die Migrationen sowie das Speichern und anschließende Lesen über den realen Datenbankadapter einschließlich Repository und Entity-DTO-Mapping.

## Abnahmekriterien

- Die gewählte Datenbank und notwendige Abweichungen sind dokumentiert.
- Schemaänderungen werden ausschließlich durch Liquibase verwaltet.
- Persistenztypen bleiben innerhalb des Adapters; die Basisregeln zu Transaktionen werden eingehalten.
- Die verpflichtenden Tests laufen im Testablauf der Basis erfolgreich; die gewählte Testdatenbankstrategie und ihre Aussagegrenzen sind dokumentiert.

## Abweichungen von der Basis

Keine.
