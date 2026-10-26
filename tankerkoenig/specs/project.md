# Projekt-Spec: tankerkoenig

Status: Fachliche Startentscheidungen sind geklärt; technische Ausgestaltung erfolgt bei Implementierung. Diese Spec beschreibt die spätere Umsetzung und beauftragt noch keine Implementierung.

## Ziel und Fachlichkeit

- Ziel: aktuelle Benzin- und Dieselpreise ausgewählter Tankstellen regelmäßig über die Tankerkönig-API abrufen und in Grafana vergleichen sowie zeitlich verfolgen.
- Projektkennung: `tankerkoenig`.
- Java-Basispaket: `de.thmweb.tankerkoenig`.
- Kraftstoffarten: Super E5, Super E10 und Diesel, soweit an der jeweiligen Tankstelle verfügbar.
- Datenquelle: Tankerkönig; der persönliche API-Key ist bereits als Umgebungsvariable `API_KEY_TANKERKOENIG` vorhanden. Die Anwendung liest ihn aus dieser Variable; Docker Compose reicht sie vom Host an den Anwendungscontainer weiter. Der Wert gehört nicht in diese Spec oder ins Repository.
- API-Übersicht: `../api.md`; dortige Architekturvorschläge sind keine bereits bestätigten Benutzerentscheidungen.

### Anwendungsfälle und fachliche Module

| Bereich | Verantwortung |
| --- | --- |
| Tankstellen | Auswahl der überwachten Tankstellen, Abruf und Aktualisierung ihrer Stammdaten, Speicherung in PostgreSQL |
| Preise | Regelmäßiger Abruf aller drei Kraftstoffarten für die ausgewählten Tankstellen, Auswertung von Verfügbarkeit und Öffnungsstatus |
| Monitoring | Bereitstellung von Preismetriken, Abrufstatus und Datenalter für Prometheus |
| Dashboard | Darstellung aktueller Preise, Vergleiche und Zeitverläufe in Grafana |

Die fachlichen Bereiche sind zunächst Verantwortlichkeiten; die genaue Package-Aufteilung wird bei der Implementierung nach der Basisarchitektur festgelegt. Separate Maven-Module sind bisher nicht erforderlich.

### Datenhaltung

- PostgreSQL speichert Tankstellenstammdaten separat von den Preiszeitreihen.
- Vorgesehene Stammdaten: externe Tankstellen-UUID, Name, Marke, Anschrift, Koordinaten und Zeitpunkt der letzten erfolgreichen Stammdatenaktualisierung.
- Öffnungszeiten und Sonderöffnungszeiten nur ergänzen, wenn die gewünschten Dashboard-Funktionen sie benötigen.
- Die externe Tankstellen-UUID identifiziert eine Station über wiederholte Importe hinweg; erneute Importe aktualisieren bestehende Datensätze ohne Duplikate.
- Prometheus speichert die durch regelmäßiges Auslesen erfassten Preiszeitreihen. Die Anwendung stellt dafür Metriken bereit; ein direktes Schreiben in Prometheus ist nicht vorgesehen.
- Preiszeitreihen werden ein Jahr aufbewahrt; für die technische Konfiguration wird dies als 365 Tage konkretisiert. Der Speicherbedarf ist anhand der tatsächlichen Stationszahl zu bemessen.
- Mehrere Orte sind konfigurierbar, jeweils mit einem konfigurierbaren Suchradius. Überschneidungen werden anhand der Tankstellen-UUID dedupliziert; die Zuordnung einer Station zu mehreren Suchorten bleibt erhalten.

### Vorerst nicht Bestandteil des Projekts — vorgeschlagene Abgrenzung

Bis zur Klärung nicht einplanen: flächendeckendes Erfassen aller deutschen Tankstellen, Import historischer CSV-Daten, automatische Fehlermeldungen an die MTS-K, Preisprognosen, Routenplanung, eigene mobile App und eigene Benutzerverwaltung. Für die erste Umsetzung sind eine Konfigurationsdatei und keine Benachrichtigungen vorgesehen; eine Verwaltungsoberfläche ist nicht Bestandteil.

## Aktive Erweiterungen

Folgende IDs sind ausdrücklich aktiviert:

| ID | Datei | Voraussetzungen |
| --- | --- | --- |
| `persistence-jpa` | `extensions/persistence-jpa.md` | Basis; PostgreSQL-Stammdatenhaltung ist fachlich erforderlich |
| `scheduled-polling` | `extensions/scheduled-polling.md` | Basis |
| `prometheus-monitoring` | `extensions/prometheus-monitoring.md` | Basis |
| `docker-compose` | `extensions/docker-compose.md` | Basis |

Die Dateipfade dieser Tabelle sind relativ zu `specs/`; damit liegen alle Erweiterungen unter dem in `AGENTS.md` vorgeschriebenen Pfad `specs/extensions/`. Keine aktive Erweiterung setzt eine weitere Erweiterungs-ID voraus; es bestehen keine Abhängigkeitszyklen.

`http-api` und `spring-security` bleiben inaktiv. Die Anwendung bietet vorerst keine eigene Fach-HTTP-API oder Benutzerverwaltung. Technische Management-Endpunkte werden durch `prometheus-monitoring` geregelt.

## Projektentscheidungen

### Bereits festgelegt

| ID | Entscheidung | Grundlage |
| --- | --- | --- |
| E-01 | Tankerkönig als externe Preisquelle; vorhandenen persönlichen API-Key ausschließlich über `API_KEY_TANKERKOENIG` übernehmen | Benutzerauftrag |
| E-02 | Alle angebotenen API-Kraftstoffarten E5, E10 und Diesel berücksichtigen | Bestehende Projekt-Spec |
| E-03 | Stammdaten in PostgreSQL, Preiszeitreihen in Prometheus, Visualisierung in Grafana | Benutzerauftrag und bestehende Projekt-Spec |
| E-04 | Maven, Java 21, Spring Boot 3, Lombok und Ports-/Adapter-Architektur als technische Grundlage | `../AGENTS.md` und `architecture.md` |
| E-05 | Konkrete stabile Spring-Boot-3-Version erst bei Projekterstellung offiziell prüfen und festschreiben | `../AGENTS.md`; noch keine Versionsprüfung durchgeführt |
| E-06 | Mehrere konfigurierbare Orte mit konfigurierbarem Radius | Benutzerentscheidung vom 04.10.2026 |
| E-07 | Alle Komponenten laufen in Docker; noch keine Infrastruktur vorhanden. Gemeinsamer Start über `scripts/docker-compose.yaml` | Benutzerentscheidung vom 04.10.2026 |
| E-08 | Gewünschtes Preisintervall: 5 Minuten; Stammdaten täglich; Historie: 1 Jahr | Benutzerentscheidung vom 04.10.2026; Abfragebudget siehe unten |
| E-09 | Zugriff zunächst ausschließlich im lokalen Netz | Benutzerentscheidung vom 04.10.2026 |
| E-10 | Kleingeschriebenes Basispaket und konsistente Erweiterungsablage | Freigabe der Anpassungsvorschläge vom 04.10.2026 |
| E-11 | JPA/Liquibase für Stammdaten; PostgreSQL-Testcontainers ersetzen die H2-Testvorgabe | Freigabe der Anpassungsvorschläge vom 04.10.2026 |
| E-12 | Hintergrundabfragen, Monitoring und Compose-Betrieb durch drei neue aktive Erweiterungen spezifizieren | Freigabe der Anpassungsvorschläge vom 04.10.2026 |
| E-13 | Interner Stationsschlüssel `idx` plus eindeutige externe Tankerkönig-UUID; Mehrfachzuordnung zu Suchorten | Freigabe der Anpassungsvorschläge vom 04.10.2026 |


### Zusammenspiel der Komponenten

```text
Tankerkönig-API → Spring-Boot-Anwendung → PostgreSQL (Stammdaten)
                         ↑
                Prometheus liest Metriken
                         ↑
                 Grafana fragt Zeitreihen ab
```

Die Anwendung wird zunächst als eine aktive Abfragerinstanz betrieben. Externe Abfragen und das Auslesen der Messwerte sind entkoppelt. Die technischen Details stehen in den aktivierten Erweiterungen; die folgenden Werte und Umfangsentscheidungen gelten projektspezifisch:

| Einstellung / Umfang | Festlegung |
| --- | --- |
| Suchgebiete | Mehrere Orte, pro Ort stabile Kennung, Bezeichnung, Suchmittelpunkt und konfigurierbarer Radius bis zur API-Grenze |
| Preisaktualisierung | Ziel: alle 5 Minuten; längere Umläufe bei mehreren notwendigen Requests akzeptiert, API-Grenzen haben Vorrang |
| Stammdaten | Täglich; Erstaufbau beim ersten Start |
| Historie | 365 Tage in Prometheus |
| Konfigurationsform | Eingebundene Datei mit Ortsbezeichnung, Koordinaten und Radius; zunächst Ettlingen (76275), 10 km. Keine zusätzliche Ortsnamenauflösung erforderlich |
| Datenaltergrenze | Nach zwei aufeinanderfolgenden ausgefallenen Aktualisierungszyklen der jeweiligen Station aus aktuellem Vergleich entfernen; Details unten |
| Prometheus-Scrape-Intervall | Bei technischer Umsetzung festzulegen; unabhängig vom externen Preisintervall |
| Startdatei | `scripts/docker-compose.yaml` |
| Dienste | Anwendung, PostgreSQL, Prometheus und Grafana; bisher keiner vorhanden |
| Zielsystem | macOS mit Docker-Laufzeit und Compose; keine Annahme über Intel oder Apple Silicon |
| Eingehender Zugriff | Grafana im lokalen Netz; interne Dienste ohne veröffentlichte Host-Ports |
| Dashboard | Aktuelle Preise, Vergleich, Verlauf, Öffnungsstatus, Datenalter und Filter nach Ort/Station/Kraftstoff gemäß aktivierter Monitoring-Erweiterung |
| Anmeldung | Entwurf: Grafanas eigene Anmeldung mit extern konfigurierten Zugangsdaten; keine eigene Java-Benutzerverwaltung |
| Benachrichtigungen | Im ersten Umfang deaktiviert; spätere Erweiterung möglich |

Vorgesehener Start aus dem Projektroot: `docker compose -f scripts/docker-compose.yaml up -d --build`.

### Initialer Suchort

```yaml
locations:
  - id: ettlingen
    name: Ettlingen
    postal-code: "76275"
    latitude: 48.94094
    longitude: 8.40763
    radius-km: 10
```

Dies ist die Vorgabe für die spätere Konfigurationsdatei, noch keine implementierte Anwendungskonfiguration. Der Suchmittelpunkt verwendet die [Ortskoordinaten von Ettlingen bei GeoNames](https://www.geonames.org/2928615/ettlingen.html), geprüft am 04.10.2026. PLZ und Name dienen der Bezeichnung; gesucht wird im Kreis um die Koordinaten, nicht entlang der PLZ-Grenzen. Weitere Orte können über zusätzliche Einträge konfiguriert werden.

### Abfrageplan und Veraltung

- Längere Umläufe sind ausdrücklich akzeptiert, wenn mehrere Requests unter Einhaltung der Anbietergrenzen erforderlich sind. Ein gemeinsamer Plan verteilt diese fair und zeigt effektiven Aktualisierungsrhythmus und tatsächliches Datenalter im Dashboard.
- Für den Start wird eine Umkreissuche je Ort mit allen Kraftstoffarten vorgesehen. Bereits gelieferte Preise verwenden; keine zusätzlichen Preisabfragen derselben Daten ohne Bedarf. Der bisher konservativ angesetzte globale Fünf-Minuten-Abstand plus Zufallsverzögerung bleibt Entwurfsgrundlage; aktuelle Anbieterbedingungen vor Implementierung abgleichen.
- Veraltung wird pro Station anhand ihrer geplanten Aktualisierungsgelegenheiten bewertet. Nach dem zweiten aufeinanderfolgenden ausgefallenen Zyklus sind alte Preise nicht mehr aktuell und werden aus aktuellen Vergleichen entfernt. Historische Werte bleiben in Prometheus.
- Ein Zyklus ist ausgefallen, wenn bis zu seinem geplanten Ende einschließlich begrenztem Request-Timeout und eingeplanter Zufallsverzögerung keine erfolgreiche Aktualisierung vorliegt. Wiederholungsversuche innerhalb desselben Zyklus zählen nicht mehrfach. Auch ein ausgebliebener Lauf muss anhand der Uhr als Ausfall erkennbar sein.
- Ein erfolgreicher Zustandsabruf setzt die Ausfallzählung zurück; `closed`, `no prices` oder ein fehlender Kraftstoffpreis ziehen betroffene Preise unmittelbar zurück. Vor dem ersten erfolgreichen Abruf gibt es keinen aktuellen Preis.
- Zwei ausgefallene Zyklen entsprechen bei einem regulären Fünf-Minuten-Zyklus ungefähr zehn Minuten ohne erfolgreiche Aktualisierung; bei mehreren Orten gilt der tatsächlich geplante Stationsrhythmus. Ein Fehler darf den Bewertungsrhythmus nicht durch ständig wachsende Wiederholungsabstände unbegrenzt verlängern.

## Abweichungen und Konfliktauflösung

1. **Basispaket bereinigt:** Der ursprüngliche Wert mit großem `W` wird durch `de.thmweb.tankerkoenig` ersetzt und entspricht damit `architecture.md`, Abschnitt „Package-Struktur“.
2. **Erweiterungsablage bereinigt:** Die übernommenen Dateien wurden nach `specs/extensions/` verschoben. Aktivierungen und Voraussetzungen sind oben vollständig aufgelistet.
3. **Persistenztests geändert:** In `extensions/persistence-jpa.md`, Abschnitte „Technische Regeln“, „Tests“ und „Abnahmekriterien“, ersetzt PostgreSQL mit Testcontainers die ursprüngliche H2-Pflicht. Grund: Migrationen und Datenzugriffe gegen das tatsächliche Datenbanksystem prüfen. Docker ist dafür Testvoraussetzung; die Maven-Testzuordnung bleibt unverändert.
4. **Monitoring-Testvertrag präzisiert:** `extensions/prometheus-monitoring.md`, Abschnitt „Abweichungen von der Basis“, konkretisiert die externe Integrationstestpflicht aus `architecture.md` durch Tests des tatsächlichen Metrik-Endpunkts. Es gibt keinen ausgehenden Prometheus-Client.
5. **Keine eigene Fach-API oder Java-Anmeldung:** Die inaktiven Erweiterungen `http-api` und `spring-security` erzeugen keine Verpflichtungen. Management-Endpunkte und Grafana-Zugriff sind separat geregelt.

## Abnahmekriterien

Für die spätere Implementierung; noch nicht erfüllt oder getestet:

- [ ] Mindestens zwei Orte mit unterschiedlichen Radien sind konfigurierbar; Überschneidungen erzeugen keine doppelten Stationen oder Preiszeitreihen.
- [ ] Der geplante Aktualisierungsrhythmus und eventuelle Abweichungen aufgrund des Abfragebudgets sind sichtbar und entsprechen der vereinbarten regelkonformen Verteilung mit gegebenenfalls längeren Umläufen.
- [ ] Stammdaten werden in PostgreSQL gespeichert und wiederholt ohne Duplikate aktualisiert.
- [ ] E5-, E10- und Dieselpreise werden bei Verfügbarkeit erfasst und korrekt einer Station zugeordnet.
- [ ] Externe Abfragen einschließlich Wiederholungen, Suche und Details halten den gemeinsamen Abfrageplan und die API-Grenzen ein.
- [ ] Prometheus-Abrufe lösen keine zusätzlichen Tankerkönig-Requests aus.
- [ ] Fehlende Preise und unbekannte Zustände erzeugen keine irreführenden Nullpreise oder falschen Statuswerte.
- [ ] Datenalter und Abfragefehler sind erkennbar; nach zwei ausgefallenen Stationszyklen werden alte Preise aus aktuellen Vergleichen entfernt, während historische Werte erhalten bleiben.
- [ ] Grafana zeigt mindestens aktuelle Preise und den seit Erfassungsbeginn aufgezeichneten Verlauf sowie Preisvergleich, Öffnungsstatus, Datenalter und Ortsfilter.
- [ ] Docker Compose übergibt `API_KEY_TANKERKOENIG` an die Anwendung. Bei fehlendem oder leerem Wert wird der Start mit einer verständlichen Meldung ohne Schlüsselwert abgebrochen.
- [ ] Zugangsdaten fehlen in Repository, Logs und Dashboard; Tankerkönig ist als Quelle genannt.
- [ ] Alle vier Komponenten starten gemeinsam über `scripts/docker-compose.yaml` auf einem Docker-Host ohne vorhandene Fachinfrastruktur.
- [ ] Grafana-Datenquelle und Dashboard sind automatisch eingerichtet und im lokalen Netz erreichbar.
- [ ] Stammdaten werden täglich aktualisiert; die Aufbewahrung in Prometheus ist auf 365 Tage eingestellt.
- [ ] Container-Neustarts erhalten vorhandene Daten über persistente Volumes.
- [ ] Installation, Konfiguration, Start und Datenaufbewahrung sind für Docker Compose dokumentiert.
- [ ] Unit- und Integrationstests erfüllen `architecture.md` sowie alle aktivierten Erweiterungen. Automatische Tests benötigen keinen echten Tankerkönig-Key und keine extern erreichbaren Produktivsysteme.
- [ ] Versionsprüfung und Maven-Prüfläufe aus `AGENTS.md` werden bei Projekterstellung durchgeführt und ihre Ergebnisse dokumentiert.

## Offene Fragen und dokumentierte Annahmen

### Geklärte Benutzerentscheidungen

- Initialer Ort: Ettlingen, PLZ 76275, Radius 10 km; weitere Orte bleiben konfigurierbar.
- Bei mehreren notwendigen API-Abfragen sind längere Umläufe akzeptiert; tatsächliche Aktualität anzeigen.
- Preise nach zwei ausgefallenen Aktualisierungszyklen aus aktuellen Vergleichen entfernen; Historie erhalten.
- Zielsystem: Mac; Betrieb vollständig in Docker über `scripts/docker-compose.yaml`.
- Weitere bestehende Festlegungen bleiben gültig: Preiszielintervall fünf Minuten, Stammdaten täglich, ein Jahr Historie, lokales Netz, API-Key aus `API_KEY_TANKERKOENIG`.

Für den ersten Projektumfang sind keine weiteren fachlichen Antworten erforderlich. Benachrichtigungen bleiben zunächst aus. Backup-Automatisierung kann später ergänzt werden.

### Technische Annahmen zur anschließenden Konkretisierung

- Zunächst eine aktive Instanz des API-Abfragers; mehrere Instanzen benötigen gemeinsame Koordination zur Vermeidung doppelter Requests.
- Preisverläufe entstehen ab Beginn unserer Messung; zwischen zwei Abrufen können Preisänderungen unbemerkt bleiben.
- Prometheus-Aufbewahrung ersetzt keine gesondert vereinbarte Backup-Strategie. Backup-/Wiederherstellungsbedarf ist mit der Betriebsumgebung festzulegen.
- Prometheus-Abfrageintervall, Zeitlimits und Zufallsverzögerung werden bei Implementierung konkretisiert; sie dürfen die vereinbarte Veraltungsregel nicht umgehen.
- Bei Implementierung die Mac-Prozessorarchitektur prüfen und passende Container-Images wählen; keine unnötige Festlegung auf `linux/amd64`.
- Die drei neuen Erweiterungen und `persistence-jpa` sind aktiviert; `http-api` und `spring-security` bleiben inaktiv.
- Nach initialem Start bleiben Preise bis zur ersten erfolgreichen Abfrage unbekannt. Entfernte Orte lösen eine Bereinigung aktiver Zuordnungen aus; historische Zeitreihen bleiben bis zum Ablauf der Retention erhalten.

## Aktualisierungshistorie der Specs

| Datum | Änderung | Stand / Herkunft |
| --- | --- | --- |
| 2026-10-04 | Projektvorlage um Fachlichkeit, Datenhaltung, technischen Entwurf, Abnahmekriterien und offene Entscheidungen ergänzt | Benutzerauftrag, vorhandene lokale Basis-Specs und `../api.md` |
| 2026-10-04 | Mehrortsuche, konfigurierbare Radien, vollständigen Docker-Compose-Betrieb unter `scripts/`, Intervalle, Jahresaufbewahrung und lokalen Zugriff übernommen; Mehrort-Abfragebudget als offenen Konflikt dokumentiert | Antworten des Benutzers |
| 2026-10-04 | Freigegebene Anpassungen umgesetzt: Erweiterungen verschoben und aktiviert, Basispaket normalisiert, PostgreSQL-Tests festgelegt, Polling-/Monitoring-/Compose-Erweiterungen ergänzt und doppelte Detailregeln zusammengeführt | Benutzerauftrag „alles so umsetzen“; dokumentierte Prüfung von Pfaden und Voraussetzungen |
| 2026-10-04 | `API_KEY_TANKERKOENIG` als verbindliche Umgebungsvariable einschließlich Compose-Weitergabe und Startvalidierung festgelegt | Benutzerauftrag |
| 2026-10-04 | Ettlingen mit 10 km Radius, längere regelkonforme Umläufe, Veraltung nach zwei ausgefallenen Zyklen und macOS als Ziel festgelegt | Antworten des Benutzers; Ortskoordinaten über GeoNames geprüft |

Vorlagenversion: in den vorliegenden Dateien nicht ausgewiesen. Es wird keine Versionsnummer oder externe Herkunft angenommen. Spätere bestätigte Entscheidungen sind mit Datum in dieser Datei nachzuführen.
