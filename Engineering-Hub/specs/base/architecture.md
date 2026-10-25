# Architekturgrundlage für Spring-Boot-Projekte

## Ziel und Begriffe

Diese Datei ist die verbindliche Quelle für allgemeine Architektur, Fehlerverträge und Testregeln künftiger Projekte. Technische Grundlage, Versionsauswahl und Arbeitsablauf sind in der `AGENTS.md` definiert (in der Sammlung unter `base/`, im Zielprojekt im Projektroot). Technologiespezifische Regeln gelten ausschließlich über die in `specs/project.md` aktivierten Erweiterungen.

Ein **fachliches Modul** beziehungsweise **Submodul** ist ein zusammenhängender fachlicher Bereich als Java-Package. Separate Maven-Module werden nur bei einem konkreten Bedarf eingeführt. Die nachfolgenden Namen sind Beispiele, keine Aufforderung, genau diese Fachlichkeit zu implementieren.

## Package-Struktur

```text
<basispaket>
├── Application.java
├── order/                                  # beispielhaftes fachliches Modul
│   ├── api/                                # bei Bedarf eingehende Schnittstellen
│   ├── contract/                           # bei Bedarf Schnittstellen für andere Module
│   ├── services/                           # Anwendungsfälle und Geschäftsregeln
│   ├── dto/
│   │   └── paymentservice/                 # bei Bedarf externe Transport-DTOs
│   └── port/
│       └── PaymentServicePort.java
└── adapter/
    └── paymentservice/
        ├── PaymentServiceAdapter.java
        ├── client/
        ├── converter/
        └── configuration/
```

Java-Package-Namen sind kleingeschrieben. Lege Packages nur an, wenn sie benötigt werden. `order`, `paymentservice` und `<basispaket>` werden durch passende Projektnamen ersetzt. Bei Maven-Multimodulprojekten gelten dieselben fachlichen Grenzen; Maven-Abhängigkeiten müssen zyklusfrei bleiben.

## Schichten innerhalb fachlicher Module

Der Aufrufweg ist grundsätzlich `api → services → port`. Die API-Schicht enthält bei Bedarf eingehende Transportlogik und Eingangsvalidierung; sie setzt keine HTTP-API voraus. Die Business-Schicht liegt im Package `services` und enthält Anwendungsfälle und Geschäftsregeln. Ports definieren die benötigten ausgehenden Schnittstellen. Die Persistenzimplementierung liegt vollständig im Datenbankadapter; dessen interne Persistenzschicht kapselt Datenzugriffe und die Umsetzung zwischen Entitäten und DTOs.

- Controller greifen nicht direkt auf Repositories, Entitäten oder externe Clients zu.
- Business-Services verwenden DTO-basierte Ports für Datenbanken und andere externe Systeme. Sie importieren keine Entitäten, Spring-Data-Repositories oder konkreten externen Adapter.
- Die öffentliche Schnittstelle des Datenbankports enthält ausschließlich DTOs und einfache Werttypen wie UUID, niemals Entitäten, Entity-Projections oder Lazy-Proxies.
- Entitäten verlassen den Datenbankadapter nicht. Das Mapping erfolgt in dessen Persistenzschicht innerhalb eines geeigneten Transaktionskontexts, bevor Daten an Business oder API zurückgegeben werden.

### Transaktionsgrenzen

- Der Business-Service legt die Transaktionsgrenze für den Datenbankteil eines Anwendungsfalls fest. Zusammengehörige Änderungen innerhalb derselben Datenbank und desselben Transaktionsmanagers müssen atomar ausgeführt werden; alle beteiligten Datenbankport-Aufrufe nehmen an derselben Transaktion teil.
- Datenbankadapter eröffnen für diese Aufrufe keine unabhängigen Transaktionen. Das Mapping von Entitäten in DTOs muss vor dem Verlassen des Transaktionskontexts abgeschlossen sein.
- Externe Netzwerkaufrufe erfolgen außerhalb der Datenbanktransaktion. Enthält ein Anwendungsfall sowohl Datenbankänderungen als auch externe Aufrufe, trennt der Service die transaktionalen Datenbankschritte von der Orchestrierung der externen Aufrufe.
- Eine lokale Datenbanktransaktion garantiert keine Atomarität über mehrere Datenbanken oder externe Systeme. Dokumentiere für solche Abläufe bei Bedarf eine Strategie für Wiederholungen, Idempotenz und fachlichen Ausgleich bereits ausgeführter Schritte. Führe zusätzliche Infrastruktur nur ein, wenn der Anwendungsfall sie benötigt.

### Modulübergreifende Schnittstellen

- Ein Modul stellt fachliche Operationen für andere Module über Interfaces unter `<fachmodul>.contract` bereit. Die Implementierungen liegen unter `<fachmodul>.services`. Lege `contract` nur an, wenn tatsächlich ein anderes Modul diese Operationen benötigt.
- Konsumierende Module verwenden ausschließlich diese Interfaces und die zugehörigen fachlichen DTOs unter `<fachmodul>.dto` sowie einfache Werttypen. Direkte Zugriffe auf fremde `services`, `api`, ausgehende `port`-Schnittstellen, Adapter oder Persistenzinternas sind ausgeschlossen.
- `contract` beschreibt angebotene fachliche Operationen; `port` beschreibt die vom eigenen Modul benötigten ausgehenden Anbindungen. Halte beide Verantwortlichkeiten getrennt und vermeide zyklische Modulabhängigkeiten.
- Vertragsrelevante Exceptions einer Modulschnittstelle gehören unter `<fachmodul>.contract.error` und sind Teil dieser Schnittstelle. Die Service-Implementierung übersetzt gegebenenfalls Fehler aus `port.error` in den dokumentierten Modulvertrag; konsumierende Module dürfen keine fremden Port-Exceptions auswerten.
- Modulschnittstellen und ihre DTOs dürfen weder externe Transport-DTOs noch technische Implementierungstypen offenlegen. Dokumentiere die relevanten Erfolgs-, Abwesenheits- und Fehlerfälle am Interface.

## Externe Systeme und Datenbanken: Ports und Adapter

Die hexagonale Grenze schützt die Fachlichkeit vor technischen Details externer Systeme:

```text
Business-Service → PaymentServicePort ← PaymentServiceAdapter → externes System
                  im fachlichen Modul   unter adapter.paymentservice
```

Der Port beschreibt fachlich benötigte Operationen. Er liegt im konsumierenden Submodul unter `port` und heißt `<NameDesExternenService>Port`. Seine Signaturen verwenden Modul-DTOs und einfache Werttypen, keine HTTP-Client-Typen, fremden SDK-Modelle oder Persistenzentitäten.

Auch eine Datenbank wird über einen Port im fachlichen Modul angebunden: `OrderDatabasePort` wird durch `adapter.orderdatabase.OrderDatabaseAdapter` implementiert. `OrderDatabase` bezeichnet dabei den logischen Namen der Datenbankanbindung. Der Business-Code hängt vom Port ab; der Adapter hängt vom Port und den Modul-DTOs ab. Das fachliche Modul importiert keine Klassen aus `adapter`.

Der Adapter implementiert den Port. Er kapselt Protokoll, Authentifizierung, Timeouts und die Übersetzung technischer Fehler. Konfiguration und Zugangsdaten werden von außen zugeführt. Fachliche Entscheidungen verbleiben im fachlichen Modul.

Bei komplexeren Anbindungen darf das Adapter-Package intern geschichtet werden, etwa in Adapter, Client und Converter. Diese Schichten dürfen keine Abhängigkeit der Business-Schicht auf die Adapterimplementierung erzeugen.

### DTOs und Converter

- Alle selbst definierten DTOs gehören zum fachlichen Submodul unter `dto`, einschließlich gegebenenfalls notwendiger Transport-DTOs für externe Systeme unter `dto.<externerservice>`.
- Controller, Business-Code und Port-Signaturen verwenden ausschließlich fachliche DTOs und einfache Werttypen, niemals Persistenzentitäten oder externe Transport-DTOs.
- Externe Transport-DTOs unter `<fachmodul>.dto.<externerservice>` bleiben trotz ihres Ablageorts Implementierungsdetails der Anbindung. Nur andere Transport-DTOs derselben Anbindung, der zugehörige Adapter einschließlich seiner internen Packages sowie die zugehörigen Tests dürfen sie verwenden. Insbesondere dürfen `api`, `services`, `contract`, `port`, fachliche DTOs und andere fachliche Module diese Typen weder importieren noch über vollqualifizierte Namen, Felder, Methodensignaturen oder Vererbung referenzieren.
- Externe Transport-DTOs dürfen nicht indirekt als Bestandteile fachlicher DTOs über einen Port zurückgegeben werden. Die Umsetzung in fachliche DTOs erfolgt im Adapter.
- Fremde SDK-Typen bleiben auf die Adapterimplementierung beschränkt und werden vor Rückgabe über den Port in fachliche DTOs umgewandelt.
- Converter externer Anbindungen liegen ausschließlich unter `adapter.<externerservice>.converter`.
- Entity-DTO-Converter liegen ebenfalls im jeweiligen Adapter, unter `adapter.<datenbankservice>.persistence.<fachbereich>.converter`. Sie dürfen weder im fachlichen Modul noch in einem gemeinsamen `services`-Package liegen.
- DTOs enthalten keine Persistenzannotationen oder Referenzen auf Entitäten. Halte sie möglichst unveränderlich, etwa als Java Records oder mit geeigneten Lombok-Annotationen.

### Fehlerverträge der Ports

- Dokumentiere für jede Port-Operation ihre relevanten Erfolgs-, Abwesenheits- und Fehlerfälle im Interface. Verwende innerhalb eines Moduls einheitliche fachliche Bedeutungen; bilde keine HTTP-Statuscodes oder herstellerspezifischen Fehlercodes im Port-Vertrag ab.
- Eine Suche nach einem möglicherweise nicht vorhandenen Einzelobjekt liefert `Optional<Dto>`, eine Suche nach mehreren Objekten eine gegebenenfalls leere Collection. Verwende dafür weder `null` noch eine Exception. Ist das Vorhandensein Voraussetzung einer Operation, signalisiere das Fehlen mit einer fachlich benannten Exception, beispielsweise `OrderNotFoundException`.
- Signalisiere einen fachlich relevanten Konflikt mit einer passenden Exception, beispielsweise `OrderConflictException`. Technische Ausfälle einer Anbindung werden als `DependencyUnavailableException`, Zeitüberschreitungen als `DependencyTimeoutException` und sonstige nicht fachlich zuordenbare Integrationsfehler als `DependencyFailureException` dargestellt. Erzeuge nur die für das Projekt tatsächlich benötigten Typen.
- Diese Exceptions sind unchecked Exceptions und liegen im konsumierenden fachlichen Modul unter `port.error`. Ihre öffentlichen Felder und eigenen Methodensignaturen enthalten ausschließlich fachliche Angaben und einfache Werttypen, keine HTTP-, SDK- oder Persistenztypen. Ein `Throwable` als interne Ursache ist zulässig.
- Adapter übersetzen bekannte technische Fehler in den dokumentierten Vertrag. Beispielsweise darf ein externer Nicht-gefunden-Status nur dann zu einem leeren Suchergebnis werden, wenn seine Bedeutung dem Suchvertrag entspricht. Ein Ausfall darf niemals als leeres Ergebnis oder erfolgreicher Aufruf erscheinen.
- Bewahre die ursprüngliche technische Exception bei einer Übersetzung als interne Ursache zur Diagnose. Business-Code und Controller dürfen weder deren Typ auswerten noch die Ursachenkette zur Steuerung verwenden. Gib technische Fehlermeldungen, Stacktraces oder Zugangsdaten nicht als API-Antwort aus.
- Die API-Schicht übersetzt die dokumentierten Fehler zentral in zum jeweiligen Eingangsprotokoll passende Antworten. Ports und Adapter legen keine HTTP-Antworten fest. Teste die Fehlerübersetzung zusammen mit der übrigen eigenen Logik gemäß Testarchitektur.

## Testarchitektur

Tests liegen unter `src/test/java` und spiegeln die Packages des Produktivcodes. Unit-Tests prüfen eigene Logik isoliert mit gemockten Abhängigkeiten. Integrationstests prüfen das Zusammenspiel produktiver Komponenten, beispielsweise eingehende Schnittstelle und Service oder Adapter und Client.

| Testart | Klassenname | Umgebung |
| --- | --- | --- |
| Unit-Test | `OrderServiceTest` | JUnit Jupiter, bei Bedarf Mockito; kein Spring-Kontext |
| Integrationstest | `PaymentServiceAdapterIntegrationTest` | gezielter Spring-Test oder mehrere reale Komponenten mit lokalem Protokoll-Stub |

- Erstelle Unit-Tests für jede Klasse mit eigener ausführbarer Logik, insbesondere Services, Controller, Adapter, Converter und eigene Persistenzimplementierungen. Teste Erfolgsfälle, relevante Grenzfälle und Fehlerfälle. Zusätzliche Integrationstests ersetzen diese Unit-Tests nicht.
- Reine Datenklassen ohne eigene Logik benötigen keine Tests für Getter, Setter oder generierten Lombok-Code. Eigene Logik in Datenklassen ist zu testen. Interfaces ohne Implementierung und ausschließlich deklarative Konfiguration benötigen keine künstlichen Unit-Tests.
- Unit-Testklassen heißen `<Klasse>Test`. Verwende JUnit Jupiter und bei Bedarf Mockito, vorzugsweise über `spring-boot-starter-test`. Unit-Tests benötigen weder Spring-Kontext noch Datenbank oder Netzwerkzugriff.
- Tests des Zusammenspiels mehrerer produktiver Komponenten heißen `<Szenario>IntegrationTest`. DTOs als Testdaten machen einen Unit-Test noch nicht zum Integrationstest.
- Mocke externe Systeme in Service- und API-Integrationstests möglichst an den Ports. Für Tests der Anbindungen gelten die nachfolgenden verbindlichen Integrationstestfälle; lokale Protokoll-Stubs verwenden dynamische Ports. Standardtests dürfen keine erreichbaren Fremdsysteme oder deren Zugangsdaten voraussetzen.
- Technologiespezifische Testumgebungen und zusätzliche Pflichtfälle werden von den aktiven Erweiterungen festgelegt. Produktive oder gemeinsam genutzte Datenbanken sind für Tests ausgeschlossen.

### Verbindliche Integrationstestfälle

Die folgenden Tests sind für vorhandene externe Anbindungen verpflichtend. Zusätzliche Pflichtfälle für HTTP und Persistenz stehen ausschließlich in den entsprechenden Erweiterungen. Erzeuge keine zusätzlichen produktiven Integrationen, Datenbanken oder Schnittstellen allein für diese Tests.

- **Externe Anbindung außerhalb der Persistenz:** Prüfe für jede solche Anbindung den tatsächlichen Adapter einschließlich Client und Converter gegen einen lokalen Protokoll-Stub. Decke mindestens einen erfolgreichen Protokollaustausch und die relevanten Fehlerübersetzungen des Port-Vertrags ab, einschließlich Ausfall oder Zeitüberschreitung, soweit vorgesehen. Ersetze in diesen Tests weder den Adapter noch seinen Client durch Mocks.

## Verbindliche Maven-Testkonfiguration

Das folgende Fragment gehört bei einer späteren Projekterstellung unter `build/plugins` der `pom.xml`. Es ist keine vollständige Projektdatei. Aktiviere beide Plugins unter `build/plugins`, nicht ausschließlich unter `pluginManagement`; Integrationstests dürfen kein zusätzlich zu aktivierendes Profil benötigen. Fehler in beiden Testarten müssen den Build fehlschlagen lassen. Die Plugin-Versionen werden durch den gewählten Spring-Boot-Parent verwaltet; falls ein anderer Parent eingesetzt wird, müssen kompatible Versionen explizit festgelegt werden.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <includes>
            <include>**/*Test.java</include>
        </includes>
        <excludes>
            <exclude>**/*IntegrationTest.java</exclude>
        </excludes>
    </configuration>
</plugin>
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-failsafe-plugin</artifactId>
    <configuration>
        <includes>
            <include>**/*IntegrationTest.java</include>
        </includes>
    </configuration>
    <executions>
        <execution>
            <id>integration-tests</id>
            <phase>integration-test</phase>
            <goals>
                <goal>integration-test</goal>
            </goals>
        </execution>
        <execution>
            <id>verify-integration-tests</id>
            <phase>verify</phase>
            <goals>
                <goal>verify</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

| Maven-Aufruf | Ausgeführte Tests |
| --- | --- |
| `mvn test` | nur Unit-Tests |
| `mvn package` | nur Unit-Tests |
| `mvn verify` | Unit-Tests und Integrationstests |
| `mvn install` | Unit-Tests und Integrationstests |

Maven durchläuft die Phasen der Reihe nach. Deshalb werden Integrationstests bereits bei `verify` ausgeführt und ihre Fehler vor `install` ausgewertet. Sie werden nicht erst an die Phase `install` gebunden. `package` kompiliert zwar sämtliche Testklassen, führt aber keine Integrationstests aus. Standardmäßig dürfen keine Skip-Optionen oder Einstellungen zum Ignorieren von Testfehlern aktiviert sein.

## Referenzen für die Projekterstellung

- [Maven Surefire: Testauswahl](https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html)
- [Maven Failsafe: Lifecycle-Konfiguration](https://maven.apache.org/surefire/maven-failsafe-plugin/usage.html)
