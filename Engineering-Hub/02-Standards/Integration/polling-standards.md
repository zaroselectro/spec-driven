# Zeitgesteuerte Datenerfassung

## Geltungsbereich

Gilt bei Auswahl für periodische externe Abfragen. Die folgenden Regeln sind verbindlich; konkrete Anbieter, Intervalle und Fristen sind Projektentscheidungen. Persistenz oder Monitoring werden dadurch nicht vorgeschrieben.

## Architektur und Ablauf

- Scheduler delegieren an Anwendungsfälle; technische Eingänge greifen nicht direkt auf Datenbank-Repositories oder externe Clients zu.
- Eine aktive Abfragerinstanz verwaltet einen gemeinsamen Plan für Suche, Stammdaten, Messwerte und Wiederholungen. Ein zusätzlicher Scheduler oder Neustart darf die Begrenzung nicht umgehen.
- Zeitbudget und Ausführung gegen eine injizierbare Uhr planen. Kein paralleler oder überlappender Lauf derselben Aufgabe. Bei verzögerten Aufgaben nicht sämtliche verpassten Ausführungen sofort nachholen.
- Ausführung fair über aktive Abfragegruppen verteilen; eine fehlerhafte Gruppe darf andere nicht dauerhaft verdrängen.
- Zufällige Verzögerung gegen synchrone Lastspitzen und begrenzte Netzwerk-Timeouts verwenden. Wiederholungen mit wachsendem Abstand und begrenzter Häufigkeit in denselben globalen Plan aufnehmen.
- Ungültige Konfiguration oder Schlüssel nicht in einer schnellen Wiederholungsschleife behandeln. Fehler sichtbar machen; andere unabhängig ausführbare Arbeiten dürfen fortgesetzt werden.
- Konfiguration validieren: eindeutige Gruppenkennungen, positive Intervalle und fachlich gültige Abfrageparameter. Kein stilles Abschneiden ungültiger Werte.
- Abfragegruppen werden beim Start eingelesen; automatische Konfigurationsänderungen zur Laufzeit sind vorerst nicht erforderlich.

## Datenverarbeitung

- Beim Start zuerst die vorhandenen Stammdaten und aktiven Zuordnungen laden beziehungsweise fehlende Daten budgetgerecht ermitteln. Ohne gültige Datenabfrage keine aktuellen Messwerte ausgeben.
- Externe Datensatzkennungen über alle Gruppen deduplizieren. Anfragegruppen halten die dokumentierte Größe des Anbieters ein.
- Der Adapter validiert HTTP-Antwort, JSON und API-Erfolg. Er übersetzt technische Fehler in die dokumentierten Fehlerverträge; fehlende Werte sind eigene fachliche Zustände.
- Nur erfolgreich ausgewertete Datensätze erhalten einen neuen Erfolgszeitpunkt. Gruppenfehler aktualisieren nicht den Erfolg aller Datensätze. Bei Teilergebnissen gültige Datensätze übernehmen und fehlende oder fehlerhafte Einträge separat behandeln.
- Datenbankänderungen nach externen Aufrufen in kurzen Transaktionen durchführen. Ein externer Timeout darf keine Datenbanktransaktion offen halten.
- Entfernte Abfragegruppen und Datensätze ohne aktive Zuordnung nicht weiter abfragen. Das Abgleichen von Suchergebnissen setzt eine erfolgreiche Suche voraus.
- Beim Herunterfahren neue Arbeit stoppen und laufende Aufgaben begrenzt beenden. Nach Neustart keine künstlich aktuellen Messwerte oder nachträglichen Beobachtungen erzeugen.

## Abfragebudget

- Zielintervall und globaler Request-Abstand sind getrennte Einstellungen und Begriffe.
- Vor Implementierung den Abfrageplan gegen die aktuellen Anbieterbedingungen prüfen. Ob längere Umläufe bei mehreren Requests akzeptabel sind, ist in `../../../../project.md` festzulegen. Effektiven Plan und Datenalter anzeigen; keine Überschreitung der Limits.
- Geplante Aktualisierungsgelegenheiten je Datensatz verfolgen. Die in `../../../../project.md` festgelegte Veraltungsgrenze anwenden; auch ausgebliebene Scheduler-Läufe erkennen. Wiederholungen innerhalb eines Zyklus nicht mehrfach zählen. Ein Fehler-Backoff darf alte Werte nicht unbegrenzt aktuell halten.

## Tests und Abnahme

- Mit kontrollierter Uhr und Zufallsquelle testen: Abstände, Wiederholungen, Nichtüberlappung, Fairness und Verhalten nach Verzögerung. Keine minutenlangen Wartezeiten in Tests.
- Überlappende Abfragegruppen, teilweise Antworten, ungültige Schlüssel, Timeouts und das Entfernen von Abfragegruppen prüfen.
- Die tatsächliche externe Anbindung einschließlich Datenkonvertierung gegen einen lokalen HTTP-Stub testen; keine echten Keys oder Live-Abfragen in Standardtests.
- API-Abrufe und effektive Aktualität müssen nachvollziehbar sein. Ein Metrikabruf startet keine Polling-Aufgabe.

