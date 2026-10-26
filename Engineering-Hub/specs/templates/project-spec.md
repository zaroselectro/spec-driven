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
- Übernommene Dateien: <Quellpfad → Zielpfad für Basis, jede Erweiterung und ausdrücklich eingebundene Standards>
- Lokale Änderungen gegenüber diesem Stand: <keine oder genaue Beschreibung>

Übernimm einen bewusst gewählten Stand. Uncommittete Vorlagenänderungen müssen zusätzlich zum Ausgangs-Commit exakt als mitgelieferter Patch oder als archivierter Dateistand referenziert werden. Behaupte keinen unveränderten Release-Stand für lokal geänderte Dateien.

## Aktive Erweiterungen

Keine. Zum Aktivieren diese Angabe durch eine vollständige Liste aus ID und Dateipfad ersetzen. Auch Voraussetzungen müssen ausdrücklich in dieser Liste stehen.

Verfügbare Erweiterungen der Sammlung sind `http-api`, `persistence-jpa`, `spring-security`, `scheduled-polling`, `prometheus-monitoring` und `docker-compose`. Diese Nennung aktiviert sie nicht. Die bedingten Voraussetzungen von `spring-security` sind bei der Auswahl zu beachten. Kafka-Erweiterungen sind mögliche spätere Ergänzungen, derzeit keine mitgelieferten Erweiterungen.

## Projektentscheidungen

Für jede aktive Erweiterung die dort geforderten offenen Entscheidungen festhalten. Bei ausschließlicher Verwendung der Basis die benötigten Entscheidungen zur Fachlichkeit und Laufzeit dokumentieren.

| Bereich oder Erweiterungs-ID | Entscheidung | Begründung |
| --- | --- | --- |
| <ausfüllen> | <ausfüllen> | <ausfüllen> |

### Bedingte Entscheidungsfelder

Nur für aktivierte Erweiterungen ausfüllen; diese Liste aktiviert nichts:

- `persistence-jpa`: produktives System und Version, Teststrategie (H2 oder Testcontainers), externe Identitäten, Import- und Zuordnungsregeln.
- `scheduled-polling`: Anbieterlimits, Gruppen und Deduplizierung, Zielintervall und globaler Request-Abstand, maximale Umlaufdauer, Veraltungsgrenze, Ausfallbewertung und Instanzkoordination.
- `prometheus-monitoring`: Metrikvertrag mit Namen/Typ/Einheit/Labels, Verfügbarkeits- und Frischeregeln, Scrape-Intervall, Retention, optionale Visualisierung und Quellenhinweise.
- `docker-compose`: Dienste, Zielbetriebssystem und Architekturen, Ports und Zugriffsnetz, Anmeldung, Secret- und Konfigurationsquellen, Volumes, Speicherbedarf und Wiederherstellung.

Konkrete Intervalle und Betriebsumfänge ausschließlich hier festlegen. Technische Detailregeln verbleiben in den Erweiterungen. Geheimniswerte gehören weder in diese Datei noch ins Repository.

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
