# Docker-Compose-Betrieb

## Geltungsbereich

Gilt bei Auswahl für einen gemeinsam gestarteten Compose-Stack. Die folgenden Regeln sind verbindlich; Dienstumfang, Zielhost, Ports und Betriebsumfang sind Projektentscheidungen. Die allgemeinen [Docker-Regeln](docker-standards.md) gelten ebenfalls und sind mitzuführen.

## Aufbau und Konfiguration

- Eine gemeinsame Startdatei und einen dokumentierten Startbefehl verwenden. Alle benötigten Dienste müssen ohne zuvor installierte Fachinfrastruktur startbar sein.
- Zielbetriebssystem, Prozessorarchitekturen und benötigte Dienste in `../../../../project.md` festlegen. Eine laufende Docker-Laufzeit mit Compose ist Voraussetzung; passende Images verwenden und keine unnötige AMD64-Emulation erzwingen.
- Anwendung aus dem Projekt bauen; Build-Kontext und Dateieinbindungen relativ zur Compose-Datei korrekt auflösen.
- Image-Versionen ausdrücklich festlegen; keine ungebundenen `latest`-Tags. Konkrete Versionen bei Implementierung prüfen und Kompatibilität dokumentieren.
- Beispielkonfiguration ohne echte Secrets liefern. Datenbank-/Grafana-Zugangsdaten extern zuführen; lokale Secret-Dateien vom Versionsmanagement ausschließen.
- Erforderliche Secrets aus der Host-Umgebung ausdrücklich nur an die benötigten Dienste weitergeben. Bei Pflichtvariablen Compose-Interpolation mit Validierung auf gesetzte, nichtleere Werte verwenden. Keine Kopie vorhandener Secrets in Dateien verlangen.
- Die Anwendung validiert erforderliche Secrets auch ohne Compose. Bei fehlenden Werten verständlich abbrechen, keine Ersatzschlüssel verwenden und keine externen Anfragen senden. Secretwerte niemals protokollieren.
- Fachliche Abfrageparameter und Intervalle in eine eingebundene Konfigurationsdatei auslagern. Neustart als anfänglichen Mechanismus zum Übernehmen geänderter Konfiguration dokumentieren.
- Bei gewähltem Monitoring-Stack Prometheus-Konfiguration, Grafana-Datenquelle und Dashboard als versionierte Dateien automatisch einbinden. Ein Erststart erfordert keine manuelle Verbindung der Dienste.

## Daten und Betrieb

- Benannte persistente Volumes für zustandsbehaftete Dienste verwenden. Gewöhnliche Neustarts oder Containerersetzung erhalten die Daten.
- Aufbewahrung der gespeicherten Daten gemäß `../../../../project.md` konfigurieren. Speicherbedarf anhand des Datenvolumens und bei Zeitreihen anhand aktiver Serien, Scrape-Intervall und Retention berücksichtigen; keine kleinere Größenbegrenzung einführen, die die zugesagte Historie unbemerkt verkürzt.
- Bereitschaftsprüfungen und begrenzte Wiederverbindungen vorsehen. Ein temporär nicht erreichbarer abhängiger Dienst darf keinen dauerhaften Startfehler verursachen.
- Interne Dienste über Compose-Dienstnamen adressieren. Nur ausdrücklich benötigte Benutzerzugänge veröffentlichen; Datenbank, Monitoring-Speicher und Anwendungsmanagement bleiben standardmäßig intern. Zugriffsnetz und Bind-Adressen in der Projekt-Spec festlegen.
- Den Zugriff auf Benutzeroberflächen ausdrücklich konfigurieren, etwa durch deren eigene Anmeldung mit extern gesetzten Zugangsdaten. Keine eigene Java-Benutzerverwaltung daraus ableiten.
- Erforderliche ausgehende Anbieterzugriffe getrennt von eingehenden Benutzerzugriffen konfigurieren.
- Start, Stop, Neustart, Logs und Wiederherstellung dokumentieren. Auf die Datenlöschung durch explizites Entfernen der Volumes hinweisen. Eine Backup-Strategie bleibt eine eigene Betriebsentscheidung.

## Prüfungen und Abnahme bei Implementierung

- Compose-Konfiguration auflösen und validieren, ohne dabei echte Secrets in veröffentlichte Ausgaben zu übernehmen.
- Reproduzierbaren Integrationstest mit lokaler Anbieter-Stub-Anbindung ermöglichen: vollständiger gewählter Stack startet und seine Integrationen funktionieren. Bei gewähltem Monitoring Anwendung, Metrikerfassung und provisionierte Dashboards gemeinsam prüfen.
- Für den Stub eine separate Testkonfiguration nutzen; keine zusätzliche produktive Fachintegration und kein echter API-Key für automatische Tests.
- Neustart mit erhaltenen Volumes prüfen: persistierte Fachdaten und gegebenenfalls Monitoring-Historie und Dashboard-Zustand bleiben vorhanden.
- Prüfen, dass keine unbeabsichtigten Host-Ports veröffentlicht werden. Fehlende Docker-Laufzeit als nicht ausgeführte Prüfung melden.
- Diese Betriebsprüfungen ergänzen den projektspezifischen Testablauf. Sie ersetzen keine Unit- oder Adapterintegrationstests.

