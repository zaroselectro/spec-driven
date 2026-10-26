# Metriken und Monitoring

## Geltungsbereich

Gilt bei Auswahl für die Bereitstellung fachlicher Messwerte an Prometheus. Die folgenden Regeln sind verbindlich; Metrikvertrag, Fristen, Retention und optionale Visualisierung sind Projektentscheidungen.

- Metrikabrufe lesen einen threadsicheren, konsistenten Snapshot ohne externe HTTP- oder Datenbankabfragen.
- Nur benötigte technische Endpunkte für interne Monitoring-Zugriffe bereitstellen; Zugriffsschutz dokumentieren.

## Metrikvertrag

- Für jede Metrik Namen im Prometheus-Expositionsformat, Typ, Einheit, Labels und Bedeutung dokumentieren. Frameworkseitige Namensumsetzung durch einen Endpoint-Test prüfen.
- Fachliche Messwerte, Verfügbarkeit, letzter Erfolgszeitpunkt und technische Fehler getrennt modellieren. Ein erfolgreicher Zustandsabruf muss keinen verfügbaren Messwert bedeuten.
- Stabile externe Identitäten verwenden; überlappende Gruppen dürfen Messzeitreihen nicht duplizieren. Gruppenbeziehungen bei Bedarf als eigene Informationsmetrik abbilden.
- Keine Messwerte, Zeitstempel, Fehlermeldungen, Request-URLs oder Secrets als Labelwerte verwenden. Labelmengen begrenzen; geänderte Informationslabels ersetzen alte Serienregistrierungen.
- Fehlende Werte und unbekannte Zustände niemals als scheinbar gültigen Nullwert ausgeben.

## Aktualität und Lebenszyklus

- Die Veraltungsregel aus `specs/project.md` bestimmt die Frische und wann bisherige Messwerte zurückgezogen werden. Die Zeitgrenze richtet sich nach dem geplanten Aktualisierungsrhythmus einschließlich begrenzter Zeitreserve; Fehler und Wiederholungen dürfen sie nicht unbegrenzt verschieben.
- Bei einem Transportfehler alte gültige Messwerte nur bis zum Ablauf dieser datensatzbezogenen Frist weiter ausgeben. Den Erfolgszeitpunkt nicht erneuern. Nach Fristablauf die Messwertserie nicht mehr ausgeben und die Verfügbarkeit auf 0 setzen.
- Bei erfolgreicher Antwort ohne gültigen Messwert den betreffenden alten Messwert unmittelbar zurückziehen. Unveränderte gültige Messwerte erhalten nach erfolgreicher Prüfung einen neuen Erfolgszeitpunkt.
- Beim Neustart bleiben aktuelle Messwerte bis zur ersten erfolgreichen Abfrage unbekannt. Historische Zeitreihen verbleiben in Prometheus; keine Wiederveröffentlichung alter Werte mit fingierter Aktualität.
- Nicht mehr überwachte Datensätze aus dem aktuellen Snapshot und den registrierten Metriken entfernen. Historische Daten bleiben bis zum Ablauf der Retention verfügbar.
- Fachlicher Erfolgszeitpunkt und Prometheus-Scrape-Zeit sind getrennt: wiederholtes Auslesen eines gecachten Messwertes ist keine neue API-Beobachtung.

## Grafana bei gewählter Visualisierung

- Datenquelle und Dashboard provisionieren. Fachlich benötigte Filter bereitstellen; beim Zusammenführen mit Gruppenzuordnungen Doppelzählungen verhindern.
- Aktuelle Messwerte mit fachgerechter Einheit und Genauigkeit, Vergleiche, Verlauf, Status und Datenalter gemäß Projektanforderungen darstellen. Fehlende Werte als unbekannt/nicht verfügbar anzeigen.
- Aktuelle Vergleiche verwenden nur frische verfügbare Werte und berücksichtigen fachliche Ausschlusszustände.
- Erforderliche Quellenhinweise anzeigen. Benachrichtigungen sind eine separate Projektentscheidung.

## Tests und Abnahme

- Den tatsächlichen Metrik-Endpunkt auf Namen, Labels, Werte und Entfernung ungültiger/veralteter Serien prüfen. Dabei keine eigene Fach-HTTP-API erzeugen.
- Nach wiederholtem Scrapen bleibt die Zahl externer API-Aufrufe unverändert.
- Gleichzeitiges Aktualisieren und Auslesen liefert keine teilweise aktualisierte Gruppe.
- Zustände vor erstem Erfolg, Fehler, Fristablauf, fehlende Messgrößen, entfernte Datensätze und geänderte Informationslabels testen.
- Frameworkseitige Endpoint- und Registry-Funktion durch Integrationstests prüfen; keine künstlichen Unit-Tests für deklarative Konfiguration.

