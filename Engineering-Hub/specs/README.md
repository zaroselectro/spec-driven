# Wiederverwendbare Projekt-Specs

Diese Sammlung definiert eine Spring-Boot-Basis und optional aktivierbare Erweiterungen. Sie enthält keine ausführbare Anwendung. Die Inhalte unter `prompts/` dokumentieren ursprüngliche Anforderungen und aktivieren keine Regeln.

## Aufbau

```text
base/
  AGENTS.md                 # technische Grundlage, Auswahlregeln und Arbeitsablauf
  architecture.md           # gemeinsame Architektur und Testregeln
extensions/
  http-api.md               # eingehende HTTP-APIs
  persistence-jpa.md        # JPA, Liquibase und wählbare Testdatenbankstrategie
  spring-security.md        # Authentifizierung und Autorisierung
  scheduled-polling.md      # koordinierte Hintergrundabfragen
  prometheus-monitoring.md  # Metriken, Aktualität und optionale Dashboards
  docker-compose.md         # reproduzierbarer lokaler Stackbetrieb
templates/
  project-spec.md           # Fachlichkeit, Auswahl und Entscheidungen pro Projekt
  extension-spec.md         # einheitliche Vorlage für neue Erweiterungen
```

Die bisherige Vorlage aus `spring-boot/` wurde in diese Struktur überführt. Verwende für neue Projekte die nachfolgende Zuordnung.

## Verwendung in einem neuen Projekt

1. Wähle einen konkreten Commit der Sammlung und die benötigten Erweiterungen.
2. Kopiere die Dateien einschließlich ausdrücklich eingebundener Standards gemäß Tabelle. Bei bestehenden Projekten vorhandene Regeln und lokale Änderungen zuerst abgleichen; nicht blind überschreiben.
3. Fülle `specs/project.md` aus, insbesondere Fachlichkeit, vollständige aktive Erweiterungsliste, Voraussetzungen, Projektentscheidungen und Vorlagenstand.
4. Prüfe fehlende Voraussetzungen und Konflikte gemäß `AGENTS.md`. Bloß kopierte Dateien sind nicht automatisch aktiv.
5. Erteile anschließend den konkreten Auftrag zur Projekterstellung.

| Quelle in der Sammlung | Ziel im Projekt |
| --- | --- |
| `base/AGENTS.md` | `AGENTS.md` |
| `base/architecture.md` | `specs/architecture.md` |
| `templates/project-spec.md` | `specs/project.md` |
| `extensions/<id>.md` (nur ausgewählte) | `specs/extensions/<id>.md` |
| `../02-Standards/<bereich>/<datei>.md` (explizit eingebundene, einschließlich ihrer Voraussetzungen) | `02-Standards/<bereich>/<datei>.md` |

Die Basis ist auch ohne Erweiterungen verwendbar und erzwingt weder Web-API noch Datenbank. Für eine HTTP-Anwendung mit relationaler Persistenz werden `http-api` und `persistence-jpa` ausdrücklich aktiviert. Deren Dateien enthalten die jeweils notwendigen Projektentscheidungen.

Für geschützte Servlet-HTTP-APIs aktiviere zusätzlich `spring-security`; für ausschließliche Method Security kann diese Erweiterung ohne `http-api` verwendet werden. Das konkrete Anmeldeverfahren und die Zugriffsmatrix werden in der Projekt-Spec festgelegt.

Hintergrunddienste können `scheduled-polling`, `prometheus-monitoring` und `docker-compose` unabhängig auswählen. Keine dieser Erweiterungen setzt eine Fach-HTTP-API, eigene Benutzerverwaltung oder relationale Datenbank voraus. Konkrete Anbieter, Metriken, Intervalle, Zielsysteme und Dienstkombinationen werden ausschließlich in der jeweiligen Projekt-Spec festgelegt. Für JPA wird auch die Teststrategie ausdrücklich gewählt; PostgreSQL kann produktionsnah mit Testcontainers geprüft werden.

## Zuständigkeit und Kombination

- [Basis und Auswahlregeln](base/AGENTS.md)
- [Allgemeine Architektur](base/architecture.md)
- [HTTP-API](extensions/http-api.md)
- [JPA-Persistenz](extensions/persistence-jpa.md)
- [Spring Security](extensions/spring-security.md)
- [Zeitgesteuerte Datenerfassung](extensions/scheduled-polling.md)
- [Prometheus-Monitoring](extensions/prometheus-monitoring.md)
- [Docker-Compose-Betrieb](extensions/docker-compose.md)
- [Projektvorlage](templates/project-spec.md)

Erweiterungen ergänzen die Basis; Abweichungen müssen konkrete Regeln benennen. Untereinander haben Erweiterungen keinen stillschweigenden Vorrang. Projektentscheidungen lösen Konflikte ausdrücklich auf; explizite Benutzeranforderungen haben Vorrang. Die vollständigen Auswahl- und Konfliktregeln werden ausschließlich in `base/AGENTS.md` gepflegt.

## Eingebundene Standards

Allgemeine technische Regeln werden unter `02-Standards/` gepflegt. Erweiterungen enthalten deren ausdrückliche Einbindung sowie die zusätzlichen Regeln der Spring-Boot-Basis. Aktivierung macht die eingebundenen Regeln im jeweiligen Geltungsbereich verbindlich; andere Standards und Erweiterungen bleiben davon unberührt.

| Erweiterung | Mitzuführende Standards relativ zum Hub beziehungsweise Zielprojektroot |
| --- | --- |
| `scheduled-polling` | `02-Standards/Integration/polling-standards.md` |
| `prometheus-monitoring` | `02-Standards/Observability/monitoring-standards.md` |
| `docker-compose` | `02-Standards/Infrastructure/compose-standards.md`, `02-Standards/Infrastructure/docker-standards.md` |
| `persistence-jpa` | `02-Standards/Data/import-standards.md`, `02-Standards/Testing/database-test-standards.md` |

Diese Zielpfade erhalten die relativen Links der Erweiterungen. Die ausgewählten Standards mit Quelle und Dateistand in `specs/project.md` aufführen. Weiterführende Wissens- oder Workflowverweise aktivieren keine zusätzlichen Regeln und sind keine technischen Voraussetzungen.

## Neue Erweiterungen hinzufügen

Verwende [extension-spec.md](templates/extension-spec.md), wähle eine eindeutige ID und lege die Datei unter `extensions/<id>.md` an. Definiere Voraussetzungen, offene Entscheidungen und zusätzliche Abnahmekriterien. Prüfe die Kombination mit der Basis und vorhandenen Erweiterungen.

Mögliche nächste Erweiterungen sind `kafka` und `kafka-streams`. Sie sind noch nicht implementiert oder aktivierbar. Eine spätere Kafka-Streams-Erweiterung kann gemeinsame Kafka-Regeln durch eine ausdrücklich deklarierte Voraussetzung `kafka` einbeziehen. Eigene Erweiterungen werden ebenfalls als Dateien in das Zielprojekt übernommen.

## Vorlagenstand und Aktualisierung

Kopiere einen festgelegten Stand ins Projekt und dokumentiere Quelle, vollständigen Commit-Hash, gegebenenfalls Release, Datum und kopierte Dateien in `specs/project.md`. Lokale Abweichungen werden dort separat festgehalten; uncommittete Quelländerungen erfordern zusätzlich einen exakt referenzierten Patch oder archivierten Dateistand.

Änderungen dieser Sammlung aktualisieren bestehende Projekte nicht automatisch. Vergleiche bei einem bewussten Update alten und neuen Stand, erhalte lokale Projektentscheidungen, prüfe die aktivierten Kombinationen und aktualisiere anschließend Vorlagenstand und Historie.
