# Projekt-Spec: <Projektname>

Diese Vorlage wird nach `specs/project.md` im Zielprojekt kopiert und konkret ausgefüllt. Platzhalter sind offene Entscheidungen und keine Implementierungsanforderungen. Bei einem ausdrücklich gewünschten freien Beispielprojekt dürfen sinnvolle Annahmen dokumentiert werden.

## Ziel und Fachlichkeit

- Ziel: <ausfüllen>
- Anwendungsfälle und fachliche Module: <ausfüllen>
- Nicht Bestandteil des Projekts: <ausfüllen>
- Projektkennung und Java-Basispaket: <ausfüllen>

## Vorlagenstand

- Quelle der zentralen Sammlung: <Repository-URL oder eindeutiger Pfad>
- Vollständiger Commit-Hash: <ausfüllen>
- Release/Tag, falls vorhanden: <optional; ersetzt nicht den Commit-Hash>
- Übernommen am: <Datum>
- Übernommene Dateien: <Quellpfad → Zielpfad für Basis und jede Erweiterung>
- Lokale Änderungen gegenüber diesem Stand: <keine oder genaue Beschreibung>

Übernimm einen bewusst gewählten Stand. Uncommittete Vorlagenänderungen müssen zusätzlich zum Ausgangs-Commit exakt als mitgelieferter Patch oder als archivierter Dateistand referenziert werden. Behaupte keinen unveränderten Release-Stand für lokal geänderte Dateien.

## Aktive Erweiterungen

Keine. Zum Aktivieren diese Angabe durch eine vollständige Liste aus ID und Dateipfad ersetzen. Auch Voraussetzungen müssen ausdrücklich in dieser Liste stehen.

Verfügbare Erweiterungen der Sammlung sind `http-api`, `persistence-jpa` und `spring-security`. Diese Nennung aktiviert sie nicht. Die bedingten Voraussetzungen von `spring-security` sind bei der Auswahl zu beachten. Kafka-Erweiterungen sind mögliche spätere Ergänzungen, derzeit keine mitgelieferten Erweiterungen.

## Projektentscheidungen

Für jede aktive Erweiterung die dort geforderten offenen Entscheidungen festhalten. Bei ausschließlicher Verwendung der Basis die benötigten Entscheidungen zur Fachlichkeit und Laufzeit dokumentieren.

| Bereich oder Erweiterungs-ID | Entscheidung | Begründung |
| --- | --- | --- |
| <ausfüllen> | <ausfüllen> | <ausfüllen> |

## Abweichungen und Konfliktauflösung

Keine. Bei Bedarf je Abweichung dokumentieren:

- Betroffene Datei, Abschnitt und Regel.
- Geltende Ersatzregel und betroffener Geltungsbereich.
- Begründung beziehungsweise explizite Benutzeranforderung.
- Bei Konflikten: alle beteiligten Erweiterungen und die gewählte Auflösung.

## Abnahmekriterien

- <Konkrete fachliche Erfolgs- und Fehlerfälle>
- <Erwartetes Start- und Laufzeitverhalten>
- Allgemeine Prüfungen der Basis und alle Abnahmekriterien der aktiven Erweiterungen sind erfüllt.

## Offene Fragen und dokumentierte Annahmen

<Keine oder konkrete Einträge; notwendige Entscheidungen vor der betroffenen Implementierung klären.>

## Aktualisierungshistorie der Specs

<Datum, bisheriger und neuer Vorlagenstand, betroffene Dateien, übernommene Änderungen und berücksichtigte lokale Abweichungen.>
