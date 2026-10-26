# Erweiterung: Docker-Compose-Betrieb

ID: `docker-compose`.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen. Der konkrete Dienstumfang wird in `../project.md` festgelegt; diese Erweiterung aktiviert keine Fachintegration.

## Aufbau und Konfiguration

- Gemeinsame Startdatei: `scripts/docker-compose.yaml`. Alle benötigten Dienste müssen ohne zuvor installierte Fachinfrastruktur startbar sein.
- Für Tankerkönig umfasst dies Anwendung, PostgreSQL, Prometheus und Grafana. Zielhost ist macOS; eine laufende Docker-Laufzeit mit Compose ist Voraussetzung. Bei Implementierung Intel beziehungsweise Apple Silicon erkennen und dazu passende Images verwenden; keine unnötige erzwungene AMD64-Emulation.
- Anwendung aus dem Projekt bauen; Build-Kontext und Dateieinbindungen relativ zur Compose-Datei korrekt auflösen. Der Startbefehl aus dem Projektroot lautet `docker compose -f scripts/docker-compose.yaml up -d --build`.
- Image-Versionen ausdrücklich festlegen; keine ungebundenen `latest`-Tags. Konkrete Versionen bei Implementierung prüfen und Kompatibilität dokumentieren.
- Beispielkonfiguration ohne echte Secrets liefern. Datenbank-/Grafana-Zugangsdaten extern zuführen; lokale Secret-Dateien vom Versionsmanagement ausschließen.
- Der Tankerkönig-Key ist in der Host-Umgebung als `API_KEY_TANKERKOENIG` vorhanden. Compose muss ihn ausdrücklich als gleichnamige Umgebungsvariable an den Anwendungscontainer weitergeben; andere Dienste benötigen ihn nicht. Keine erneute Eingabe oder Kopie in eine Konfigurationsdatei verlangen.
- Vorgesehener Eintrag unter `environment` des Anwendungsdienstes: `API_KEY_TANKERKOENIG: "${API_KEY_TANKERKOENIG:?API_KEY_TANKERKOENIG muss gesetzt und nicht leer sein}"`. Dies ist eine Vorgabe für die spätere Compose-Datei, kein echter Schlüsselwert.
- Auch bei einem Start ohne Compose validiert die Anwendung die Variable auf einen nichtleeren Wert. Bei fehlendem oder leerem Wert den Start mit verständlicher Meldung abbrechen; keinen Ersatzschlüssel verwenden und keinen API-Aufruf senden. Den Schlüssel niemals protokollieren.
- Orte, Radien und Intervalle in eine eingebundene Konfigurationsdatei auslagern. Neustart als anfänglichen Mechanismus zum Übernehmen geänderter Orte dokumentieren.
- Prometheus-Konfiguration, Grafana-Datenquelle und Dashboard als versionierte Dateien automatisch einbinden. Ein Erststart erfordert keine manuelle Verbindung der Dienste.

## Daten und Betrieb

- Benannte persistente Volumes für PostgreSQL, Prometheus und Grafana verwenden. Gewöhnliche Neustarts oder Containerersetzung erhalten die Daten.
- Aufbewahrung der Preiszeitreihen gemäß `project.md` konfigurieren. Speicherbedarf anhand aktiver Serien, Scrape-Intervall und Retention berücksichtigen; keine kleinere Größenbegrenzung einführen, die die zugesagte Historie unbemerkt verkürzt.
- Bereitschaftsprüfungen und begrenzte Wiederverbindungen vorsehen. Ein temporär nicht erreichbarer abhängiger Dienst darf keinen dauerhaften Startfehler verursachen.
- Interne Dienste über Compose-Dienstnamen adressieren. Standardmäßig nur den benötigten Grafana-Port im lokalen Netz veröffentlichen; Datenbank, Prometheus und Anwendungsmanagement bleiben intern.
- Den Zugriff auf Grafana ausdrücklich konfigurieren; Vorschlag für dieses Projekt ist Grafanas eigene Anmeldung mit extern gesetzten Zugangsdaten. Keine eigene Java-Benutzerverwaltung daraus ableiten.
- Externen Internetzugriff der Anwendung auf Tankerkönig ermöglichen; „lokales Netz“ bezieht sich auf eingehende Benutzerzugriffe.
- Start, Stop, Neustart, Logs und Wiederherstellung dokumentieren. Auf die Datenlöschung durch explizites Entfernen der Volumes hinweisen. Eine Backup-Strategie bleibt eine eigene Betriebsentscheidung.

## Prüfungen und Abnahme bei Implementierung

- Compose-Konfiguration auflösen und validieren, ohne dabei echte Secrets in veröffentlichte Ausgaben zu übernehmen.
- Reproduzierbaren Integrationstest mit lokaler Tankerkönig-Stub-Anbindung ermöglichen: vollständiger Stack startet, Anwendung liefert Metriken, Prometheus erfasst sie, Grafana verwendet die bereitgestellte Datenquelle und das Dashboard.
- Für den Stub eine separate Testkonfiguration nutzen; keine zusätzliche produktive Fachintegration und kein echter API-Key für automatische Tests.
- Neustart mit erhaltenen Volumes prüfen: Stammdaten, Prometheus-Historie und Grafana-Zustand bleiben vorhanden.
- Prüfen, dass keine unbeabsichtigten Host-Ports veröffentlicht werden. Fehlende Docker-Laufzeit als nicht ausgeführte Prüfung melden.
- Diese Betriebsprüfungen ergänzen den Maven-Testablauf. Sie ersetzen keine Unit- oder Adapterintegrationstests.

## Abweichungen von der Basis

Keine. Diese Erweiterung ergänzt Bereitstellung und Betriebsprüfungen.
