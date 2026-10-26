# Erweiterung: Prometheus-Monitoring

ID: `prometheus-monitoring`.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen. Eine fachliche HTTP-API oder Spring Security wird hierdurch nicht aktiviert.

## Technische Anbindung

- Für die spätere Spring-Boot-Anwendung Actuator und Micrometer mit Prometheus-Anbindung verwenden; kompatible Versionen über das Spring-Boot-Dependency-Management bestimmen.
- Nur benötigte Management-Endpunkte bereitstellen: Health und Prometheus. Sie sind für die interne Containerkommunikation vorgesehen; keine pauschale Veröffentlichung aller Actuator-Endpunkte.
- Fachliche Services veröffentlichen Messzustände über einen DTO-basierten Port; die technische Umsetzung liegt unter `adapter.monitoring`. Micrometer-Typen verlassen diesen Adapter nicht.
- Metrikabrufe lesen ausschließlich einen threadsicheren, konsistenten Snapshot. Keine externen HTTP- oder Datenbankabfragen beim Auslesen.

## Metrikvertrag

Die folgenden Namen bezeichnen die von Prometheus sichtbaren Zeitreihen. Konkrete Framework-Registrierung und Namensumsetzung sind bei Implementierung durch einen Endpoint-Test zu prüfen.

| Metrik | Typ / Labels | Bedeutung |
| --- | --- | --- |
| `tankerkoenig_fuel_price_eur` | Gauge; `station_id`, `fuel` | Gültiger Preis in Euro je Liter; `fuel` ist `e5`, `e10` oder `diesel` |
| `tankerkoenig_station_open` | Gauge; `station_id` | 1 offen, 0 geschlossen; bei unbekanntem oder veraltetem Zustand weglassen |
| `tankerkoenig_price_available` | Gauge; `station_id`, `fuel` | 1 bei frischem gültigem Preis, sonst 0 für aktiv überwachte Stationen |
| `tankerkoenig_last_success_timestamp_seconds` | Gauge; `station_id` | Letzte erfolgreich ausgewertete Preisantwort als Unix-Zeit; vor erstem Erfolg weglassen |
| `tankerkoenig_api_errors_total` | Counter; `operation` | Fehlgeschlagene API-Versuche; Operation aus begrenzter Liste `list`, `prices`, `detail` |
| `tankerkoenig_station_info` | Gauge mit Wert 1; `station_id`, `name`, `brand`, `place` | Aktuelle Stationsinformationen |
| `tankerkoenig_station_location_info` | Gauge mit Wert 1; `station_id`, `location_id` | Aktive Zuordnung zum konfigurierten Suchort |

- Stations-ID meint immer die externe Tankerkönig-UUID. Preiszeitreihen tragen keine Ortszuordnung, damit Überschneidungen sie nicht duplizieren.
- Keine Preise, Zeitstempel, Fehlermeldungen, Request-URLs oder Secrets als Labelwerte verwenden. Geänderte Informationslabels ersetzen alte Serienregistrierungen.
- Ein erfolgreicher Abruf mit `closed` oder `no prices` ist ein erfolgreicher Zustandsabruf, aber kein gültiger Preis. Boolesches `false`, fehlende Preise und Fehler ergeben niemals einen Preis null.

## Aktualität und Lebenszyklus

- Die Veraltungsregel aus `project.md` bestimmt die Frische: nach zwei aufeinanderfolgenden ausgefallenen Aktualisierungszyklen je Station den bisherigen Preis zurückziehen. Die Zeitgrenze richtet sich nach dem geplanten Stationsrhythmus einschließlich begrenzter Zeitreserve; Fehler und Wiederholungen dürfen sie nicht unbegrenzt verschieben.
- Bei einem Transportfehler alte gültige Preise nur bis zum Ablauf dieser stationsbezogenen Frist weiter ausgeben. Den Erfolgszeitpunkt nicht erneuern. Nach Fristablauf die Preisserie nicht mehr ausgeben und die Verfügbarkeit auf 0 setzen.
- Bei erfolgreicher Antwort ohne gültigen Preis den betreffenden alten Preis unmittelbar zurückziehen. Unveränderte gültige Preise erhalten nach erfolgreicher Prüfung einen neuen Erfolgszeitpunkt.
- Beim Neustart bleiben aktuelle Preise bis zur ersten erfolgreichen Abfrage unbekannt. Historische Zeitreihen verbleiben in Prometheus; keine Wiederveröffentlichung alter Werte mit fingierter Aktualität.
- Nicht mehr überwachte Stationen aus dem aktuellen Snapshot und den registrierten Metriken entfernen. Historische Daten bleiben bis zum Ablauf der Retention verfügbar.
- Fachlicher Erfolgszeitpunkt und Prometheus-Scrape-Zeit sind getrennt: wiederholtes Auslesen eines gecachten Preises ist keine neue API-Beobachtung.

## Grafana

- Datenquelle und Dashboard provisionieren. Filter nach Suchort, Station und Kraftstoff ermöglichen; beim Zusammenführen mit Ortszuordnungen Doppelzählungen verhindern.
- Aktuelle Preise mit drei Nachkommastellen, Preisvergleich, Verlauf, Öffnungsstatus und Datenalter darstellen. Fehlende Werte als unbekannt/nicht verfügbar anzeigen.
- Vergleiche aktueller Preise verwenden nur frische verfügbare Werte und kennzeichnen beziehungsweise filtern geschlossene Stationen nachvollziehbar.
- Quellenhinweis auf Tankerkönig anzeigen. Benachrichtigungen sind nicht automatisch Bestandteil dieser Erweiterung.

## Tests und Abnahme

- Den tatsächlichen Metrik-Endpunkt auf Namen, Labels, Werte und Entfernung ungültiger/veralteter Serien prüfen. Dabei keine eigene Fach-HTTP-API erzeugen.
- Nach wiederholtem Scrapen bleibt die Zahl externer API-Aufrufe unverändert.
- Gleichzeitiges Aktualisieren und Auslesen liefert keine teilweise aktualisierte Gruppe.
- Zustände vor erstem Erfolg, Fehler, Fristablauf, fehlende Kraftstoffarten, entfernte Stationen und geänderte Informationslabels testen.
- Frameworkseitige Endpoint- und Registry-Funktion durch Integrationstests prüfen; keine künstlichen Unit-Tests für deklarative Konfiguration.

## Abweichungen von der Basis

Die Integrationstestpflicht für externe Anbindungen in `architecture.md`, Abschnitt „Verbindliche Integrationstestfälle“, wird für diese passive Monitoring-Anbindung durch Tests des tatsächlichen Metrik-Endpunkts erfüllt. Ein ausgehender Prometheus-Client oder externer Protokoll-Stub ist hierfür nicht erforderlich. Fachliche Ports und DTO-Grenzen bleiben unverändert.
