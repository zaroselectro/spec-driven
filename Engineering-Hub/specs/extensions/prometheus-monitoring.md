# Erweiterung: Prometheus-Monitoring

ID: `prometheus-monitoring`.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen. Eine fachliche HTTP-API oder Spring Security wird hierdurch nicht aktiviert.

## Technische Anbindung

- Für die spätere Spring-Boot-Anwendung Actuator und Micrometer mit Prometheus-Anbindung verwenden; kompatible Versionen über das Spring-Boot-Dependency-Management bestimmen.
- Nur benötigte Management-Endpunkte bereitstellen: Health und Prometheus. Sie sind für interne Monitoring-Zugriffe vorgesehen; keine pauschale Veröffentlichung aller Actuator-Endpunkte.
- Fachliche Services veröffentlichen Messzustände über einen DTO-basierten Port; die technische Umsetzung liegt unter `adapter.monitoring`. Micrometer-Typen verlassen diesen Adapter nicht.

## Offene Projektentscheidungen

In `specs/project.md` festlegen: Messgrößen und Einheiten, Metriknamen, Typen, begrenzte Labels und Identitäten, Datenaltergrenze, Ausfallbewertung, Scrape-Intervall, Retention sowie benötigte Dashboards und Zugriffsschutz. Grafana ist eine optionale Projektentscheidung; diese Erweiterung aktiviert weder Polling noch Docker Compose.

## Eingebundener Standard

Bei Aktivierung gelten zusätzlich die Regeln aus [monitoring-standards](../../02-Standards/Observability/monitoring-standards.md) für den berührten Bereich. Die Datei muss beim Übernehmen mitgeführt werden; die Kopierzuordnung steht in der README der Sammlung. Dies aktiviert keine weitere Erweiterung. Projektentscheidungen und begründete Abweichungen stehen in `specs/project.md`.

## Abweichungen von der Basis

Die Integrationstestpflicht für externe Anbindungen in `specs/architecture.md`, Abschnitt „Verbindliche Integrationstestfälle“, wird für diese passive Monitoring-Anbindung durch Tests des tatsächlichen Metrik-Endpunkts erfüllt. Ein ausgehender Prometheus-Client oder externer Protokoll-Stub ist hierfür nicht erforderlich. Fachliche Ports und DTO-Grenzen bleiben unverändert.
