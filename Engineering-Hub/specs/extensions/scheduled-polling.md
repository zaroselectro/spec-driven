# Erweiterung: Zeitgesteuerte Datenerfassung

ID: `scheduled-polling`.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen. Persistenz und Monitoring werden separat aktiviert, wenn fachlich benötigt.

## Zuständigkeit

Diese Erweiterung regelt die Ausführung von Hintergrundabfragen. Abfragegruppen, Zielintervalle, zulässige Aktualisierungsdauer und Konfigurationsquellen stehen in `specs/project.md`. Grenzen externer APIs sind bei Implementierung anhand der aktuellen Anbieterbedingungen zu prüfen.

## Offene Projektentscheidungen

In `specs/project.md` festlegen: Anbieter und Limits, Abfragegruppen und Identitäten, Intervalle, maximale Datenalter, Ausfallbewertung, Konfigurationsquelle und Anzahl aktiver Instanzen. Mehrere Instanzen benötigen gemeinsame Koordination des Abfragebudgets.

## Eingebundener Standard

Bei Aktivierung gelten zusätzlich die Regeln aus [polling-standards](../../02-Standards/Integration/polling-standards.md) für den berührten Bereich. Die Datei muss beim Übernehmen mitgeführt werden; die Kopierzuordnung steht in der README der Sammlung. Dies aktiviert keine weitere Erweiterung. Projektentscheidungen und begründete Abweichungen stehen in `specs/project.md`.

## Spring-Boot-Anbindung und Abnahme

- Scheduler-Eingänge liegen unter `<fachmodul>.api.scheduling` und delegieren an Services. Services verwenden DTO-basierte Ports für externe Systeme; Fehlerverträge der Basis gelten.
- Den tatsächlichen Adapter mit Client und Converter gegen einen lokalen HTTP-Stub integrieren und testen.
- Testzuordnung und Maven-Ablauf richten sich nach der Basis; die Prüfungen des eingebundenen Standards sind zusätzlich zu erfüllen.

## Abweichungen von der Basis

Keine.
