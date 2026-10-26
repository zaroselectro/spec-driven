# Erweiterung: JPA-Persistenz

## Zweck und Geltungsbereich

ID: `persistence-jpa`. Gilt ausschließlich bei ausdrücklicher Aktivierung für relationale Persistenz mit Spring Data JPA und Liquibase. Aktiviert weder HTTP noch andere Erweiterungen.

## Voraussetzungen

- Basis-Specs; keine weiteren Erweiterungen.
- Eine fachliche Anforderung an relationale Persistenz.

## Offene Projektentscheidungen

In `specs/project.md` festlegen: produktives Datenbanksystem, Datenbankanbindungen, persistierte Fachbereiche, Konfigurationsquellen und notwendige herstellerspezifische Abweichungen. Zugangsdaten werden extern zugeführt, nicht in Specs hinterlegt.

## Technische Regeln

Eine Datenbank wird nur eingeführt, wenn sie fachlich benötigt wird. Schemaänderungen erfolgen über Liquibase; für den Datenzugriff wird möglichst Spring Data verwendet. Für relationale Datenbanken gilt:

- Ergänze die benötigten JPA-, Liquibase- und Datenbanktreiber-Dependencies sowie Testcontainers mit PostgreSQL-Unterstützung für Integrationstests. Versionsverwaltung erfolgt gemäß Basis.
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

## Tests

- Testdaten und Testkonfiguration liegen unter `src/test/resources`. Isoliere Testdaten durch Rollbacks oder gezieltes Aufräumen.
- PostgreSQL ist die Testdatenbank für Persistenzintegrationstests. Verwende eine isolierte, kurzlebige Testcontainers-Instanz mit zur produktiven PostgreSQL-Hauptversion passender Version; diese ist bei Implementierung festzulegen. Eine verfügbare Docker-Laufzeit ist Voraussetzung dieser Tests.
- Die Unit-Test- und Maven-Regeln der Basis gelten zusätzlich.
- Datenbanktests führen die regulären Liquibase-Migrationen gegen die isolierte PostgreSQL-Instanz aus. H2 ist weder erforderlich noch Ersatz für diese Tests. Produktive oder gemeinsam genutzte Datenbanken sind ausgeschlossen.
- Fehlt die Docker-Laufzeit, muss dies als nicht ausgeführte Integrationstestprüfung erkennbar sein; Tests dürfen nicht unbemerkt übersprungen oder als erfolgreich gemeldet werden.
- Prüfe für jede Datenbankanbindung die Migrationen sowie das Speichern und anschließende Lesen über den realen Datenbankadapter einschließlich Repository und Entity-DTO-Mapping.

## Abnahmekriterien

- Die gewählte Datenbank und notwendige Abweichungen sind dokumentiert.
- Schemaänderungen werden ausschließlich durch Liquibase verwaltet.
- Persistenztypen bleiben innerhalb des Adapters; die Basisregeln zu Transaktionen werden eingehalten.
- Die verpflichtenden PostgreSQL-Integrationstests laufen im Testablauf der Basis erfolgreich; sie benötigen außer Docker keine bestehende Datenbank.

## Abweichungen von der Basis

Keine.

## Projektausprägung Tankerkönig

- Eine PostgreSQL-Anbindung speichert Stammdaten und Suchortzuordnungen, keine Preiszeitreihen.
- Tankstellen erhalten den internen UUID-Primärschlüssel `idx`. Die externe Tankerkönig-UUID wird in einem eigenen Feld mit eindeutiger Datenbankbedingung gespeichert. Wiederholte Importe verwenden diese externe ID zum Aktualisieren.
- Ein Suchort besitzt eine stabile konfigurierte Kennung. Die Zuordnung zwischen Suchort und Station ist mehrfach möglich; jedes Paar darf nur einmal vorkommen. Überschneidende Suchgebiete erzeugen keine doppelten Stationen.
- Stationen speichern Name, Marke, Adresse, Koordinaten und den Zeitpunkt des letzten erfolgreichen Stammdatenimports. Postleitzahlen werden als Zeichenfolge behandelt.
- Suchortzuordnungen werden nur nach einer erfolgreichen vollständigen Suche des jeweiligen Ortes abgeglichen. Ein Fehler darf nicht als leere Suche behandelt werden und keine Zuordnungen löschen.
- Entfällt ein Suchort, werden dessen aktive Zuordnungen aufgehoben. Eine Station bleibt überwacht, solange sie mindestens einem aktiven Suchort zugeordnet ist. Stammdaten müssen dabei nicht physisch gelöscht werden.
- Ergänze Integrationstests für wiederholte Importe, eindeutige externe IDs, überlappende Suchorte und das Entfernen einzelner Zuordnungen.

## Änderung gegenüber der übernommenen Vorlage

Die ursprüngliche H2-Pflicht in „Technische Regeln“, „Tests“ und „Abnahmekriterien“ wird durch PostgreSQL-Integrationstests mit Testcontainers ersetzt. Die allgemeinen Maven- und Architekturregeln bleiben unverändert. Grundlage: Benutzerfreigabe der Anpassungsvorschläge vom 04.10.2026.
