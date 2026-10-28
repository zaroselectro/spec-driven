# Erweiterung: Docker-Compose-Betrieb

ID: `docker-compose`.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen. Der konkrete Dienstumfang wird in `../../../../project.md` festgelegt; diese Erweiterung aktiviert keine Fachintegration.

## Offene Projektentscheidungen

In `../../../../project.md` festlegen: Dienste, Zielhost und Architekturen, benötigte Host-Ports, Zugriffsnetz und Anmeldung, Konfigurations- und Secretquellen, Retention, Speicherbedarf und Backup-/Wiederherstellungsbedarf. Monitoring-Regeln gelten nur, wenn der entsprechende Stack gewählt ist.

## Eingebundener Standard

Bei Aktivierung gelten zusätzlich die Regeln aus [compose-standards](../../02-Standards/Infrastructure/compose-standards.md) für den berührten Bereich. Die Datei muss beim Übernehmen mitgeführt werden; die Kopierzuordnung steht in der README der Sammlung. Dies aktiviert keine weitere Erweiterung. Projektentscheidungen und begründete Abweichungen stehen in `../../../../project.md`.

## Projektanbindung und Abnahme

- Gemeinsame Startdatei: `scripts/docker-compose.yaml`.
- Start aus dem Projektroot: `docker compose -f scripts/docker-compose.yaml up -d --build`.
- Betriebsprüfungen des eingebundenen Standards ergänzen den Maven-Testablauf der Basis; sie ersetzen keine Unit- oder Adapterintegrationstests.

## Abweichungen von der Basis

Keine.
