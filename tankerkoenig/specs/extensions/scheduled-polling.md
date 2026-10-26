# Erweiterung: Zeitgesteuerte Datenerfassung

ID: `scheduled-polling`.

## Voraussetzungen

Basis-Specs; keine weiteren Erweiterungen. Persistenz und Monitoring werden separat aktiviert, wenn fachlich benötigt.

## Zuständigkeit

Diese Erweiterung regelt die Ausführung von Hintergrundabfragen. Orte, Radien, Zielintervalle und zulässige Aktualisierungsdauer stehen in `../project.md`. Grenzen der externen API sind anhand von `../../api.md` und bei Implementierung der aktuellen Anbieterbedingungen zu berücksichtigen.

## Architektur und Ablauf

- Scheduler-Eingänge liegen unter `<fachmodul>.api.scheduling` und delegieren an Services. Services verwenden DTO-basierte Ports für externe Systeme.
- Eine aktive Abfragerinstanz verwaltet einen gemeinsamen Plan für Suche, Stammdaten, Preise und Wiederholungen. Ein zusätzlicher Scheduler oder Neustart darf die Begrenzung nicht umgehen.
- Zeitbudget und Ausführung gegen eine injizierbare Uhr planen. Kein paralleler oder überlappender Lauf derselben Aufgabe. Bei verzögerten Aufgaben nicht sämtliche verpassten Ausführungen sofort nachholen.
- Ausführung fair über aktive Orte beziehungsweise Stationsgruppen verteilen; eine fehlerhafte Gruppe darf andere nicht dauerhaft verdrängen.
- Zufällige Verzögerung gegen synchrone Lastspitzen und begrenzte Netzwerk-Timeouts verwenden. Wiederholungen mit wachsendem Abstand und begrenzter Häufigkeit in denselben globalen Plan aufnehmen.
- Ungültige Konfiguration oder Schlüssel nicht in einer schnellen Wiederholungsschleife behandeln. Fehler sichtbar machen; andere unabhängig ausführbare Arbeiten dürfen fortgesetzt werden.
- Konfiguration validieren: eindeutige Ortskennungen, gültige Koordinaten, positiver zulässiger Radius und positive Intervalle. Kein stilles Abschneiden ungültiger Werte.
- Orte werden beim Start eingelesen; automatische Konfigurationsänderungen zur Laufzeit sind vorerst nicht erforderlich.

## Datenverarbeitung

- Beim Start zuerst die vorhandenen Stammdaten und aktiven Zuordnungen laden beziehungsweise fehlende Daten budgetgerecht ermitteln. Ohne gültige Preisabfrage keine aktuellen Preise ausgeben.
- Stations-UUIDs über alle Orte deduplizieren. Preisgruppen halten die dokumentierte Größe des Anbieters ein.
- Der Adapter validiert HTTP-Antwort, JSON und API-Erfolg. Er übersetzt technische Fehler in die Fehlerverträge der Basis; fehlende Preise sind eigene fachliche Zustände.
- Nur erfolgreich ausgewertete Stationen erhalten einen neuen Erfolgszeitpunkt. Gruppenfehler aktualisieren nicht den Erfolg aller Stationen. Bei Teilergebnissen gültige Stationen übernehmen und fehlende oder fehlerhafte Einträge separat behandeln.
- Datenbankänderungen nach externen Aufrufen in kurzen Transaktionen durchführen. Ein externer Timeout darf keine Datenbanktransaktion offen halten.
- Entfernte Orte und Stationen ohne aktive Zuordnung nicht weiter abfragen. Das Abgleichen von Suchergebnissen setzt eine erfolgreiche Suche voraus.
- Beim Herunterfahren neue Arbeit stoppen und laufende Aufgaben begrenzt beenden. Nach Neustart keine künstlich aktuellen Messwerte oder nachträglichen Preisbeobachtungen erzeugen.

## Abfragebudget

- Zielintervall und globaler Request-Abstand sind getrennte Einstellungen und Begriffe.
- Vor Implementierung den Mehrort-Plan gegen die aktuellen Anbieterbedingungen prüfen. Längere Umläufe bei mehreren Requests sind gemäß `project.md` akzeptiert. Effektiven Plan und Datenalter anzeigen; keine Überschreitung der Limits.
- Geplante Aktualisierungsgelegenheiten je Station verfolgen. Nach zwei ausgefallenen Zyklen die Veraltungsregel aus `project.md` anwenden; auch ausgebliebene Scheduler-Läufe erkennen. Wiederholungen innerhalb eines Zyklus nicht mehrfach zählen. Ein Fehler-Backoff darf alte Preise nicht unbegrenzt aktuell halten.

## Tests und Abnahme

- Mit kontrollierter Uhr und Zufallsquelle testen: Abstände, Wiederholungen, Nichtüberlappung, Fairness und Verhalten nach Verzögerung. Keine minutenlangen Wartezeiten in Tests.
- Überschneidende Orte, teilweise Antworten, ungültige Schlüssel, Timeouts und das Entfernen von Orten prüfen.
- Den realen externen Adapter mit Client und Converter gegen einen lokalen HTTP-Stub testen; keine echten Keys oder Live-Abfragen in Standardtests.
- API-Abrufe und effektive Aktualität müssen nachvollziehbar sein. Ein Prometheus-Abruf startet keine Polling-Aufgabe.

## Abweichungen von der Basis

Keine. Diese Erweiterung konkretisiert technische Eingänge und Fehlerbehandlung für Hintergrundaufgaben.
