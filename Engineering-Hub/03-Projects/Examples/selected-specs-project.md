# Beispiel B: Neues Projekt ohne agentischen Ablauf, mit ausgewählten Specs

**Demonstration:** Du entwickelst einen kleinen `catalog-service` selbst und möchtest nur Vorgaben für Java, REST und Tests übernehmen. Eine KI soll bei Bedarf einzelne Fragen beantworten oder Codevorschläge liefern; sie steuert weder Projektplanung noch Dateibearbeitung oder Testausführung.

## 1. Auswahl bewusst begrenzen

Für dieses Beispiel nur drei unabhängige allgemeine Regeldateien auswählen:

| Quelle im Hub | Projektlokale Kopie |
| --- | --- |
| `02-Standards/Java/java-standards.md` | `specs/selected/java-standards.md` |
| `02-Standards/API/rest-standards.md` | `specs/selected/rest-standards.md` |
| `02-Standards/Testing/testing-standards.md` | `specs/selected/testing-standards.md` |

Damit werden **nicht** automatisch die Spring-Boot-Basis, Lombok, Liquibase, Kafka, Rollen oder Teamworkflows übernommen. Die Regeln gelten weiterhin nur für die berührten Technologien. Erwähnungen von Mockito oder Cucumber erzwingen keine unnötigen Werkzeuge.

### Wichtig bei Auswahl von Erweiterungen

Die Dateien unter `specs/extensions/` sind im Unterschied zu den allgemeinen Standards **nicht unabhängig**: Sie setzen die Basis voraus; `spring-security` setzt bei einer eingehenden HTTP-API zusätzlich `http-api` voraus. Wenn du diese Erweiterungen auswählst, musst du die deklarierten Voraussetzungen ebenfalls übernehmen und aktivieren. Wenn du deren Basis nicht möchtest, nutze allgemeine Standards oder formuliere eine eigene, klar begrenzte Projekt-Spec. Eine Erweiterung nicht einfach als isolierte Regeldatei behandeln.

## 2. Projekt selbst anlegen

In deiner IDE ein neues Softwareprojekt erstellen. Sprache, Framework, Buildwerkzeug und konkrete Versionen selbst wählen und dokumentieren; Java 21 gilt für dieses Beispiel durch die ausgewählte Java-Regel. Eine mögliche Dokumentstruktur ist:

```text
catalog-service/
├── README.md
├── specs/
│   ├── project.md
│   ├── selection.md
│   └── selected/
│       ├── java-standards.md
│       ├── rest-standards.md
│       └── testing-standards.md
└── src/                     # von dir angelegter Anwendungscode und Tests
```

Keine `AGENTS.md` ist für diesen manuellen Ablauf erforderlich. Vorhandene Projektregeln bleiben gültig; sie dürfen durch einen KI-Prompt nicht außer Kraft gesetzt werden. Quellcode und Prüfbefehle richten sich nach deinem gewählten Buildsystem.

## 3. Kleine fachliche Spec schreiben

Beispielinhalt für `specs/project.md` — keine reale Anforderung:

```markdown
# Catalog Service — Demonstration

## Ziel
Ein lokal gestarteter HTTP-Dienst liefert einen festen Beispielkatalog.

## Umfang
GET /products liefert eine JSON-Liste mit id und name.
Kein schreibender Endpunkt, keine Datenbank, kein Kafka, kein Frontend.
Nur lokale Entwicklung; keine öffentliche Bereitstellung.

## Akzeptanzkriterien
- GET /products antwortet mit 200 und application/json.
- Jeder Listeneintrag besitzt eine eindeutige id und einen nicht leeren name.
- Eine leere Produktliste wird als [] zurückgegeben.
- Automatisierte Tests prüfen Antwort und leere Liste.

## Technische Entscheidungen
Java: 21.
Framework, konkrete Version und Buildwerkzeug: vor Umsetzung selbst eintragen.
Build-/Testbefehle: nach Einrichtung konkret eintragen.
Verbindliche Regeldateien und Übernahmestand: siehe selection.md.
```

## 4. Auswahl und Herkunft festhalten

In `specs/selection.md` die drei tatsächlich kopierten Dateien aufführen, inklusive Quelle, Übernahmedatum, SHA-256 beziehungsweise überprüfbarem Versionsstand und lokalen Änderungen. Festhalten: „Nur die aufgeführten Regeldateien gelten; keine Erweiterung der Spring-Boot-Sammlung aktiviert.“ Änderungen am Hub aktualisieren die Kopien nicht automatisch.

Dieses freie Auswahlmanifest gehört zu diesem manuellen Beispiel. Es ersetzt nicht die Aktivierungsliste in `specs/project.md`, falls du später zur vollständigen Spring-Boot-Basis wechselst.

## 5. KI optional für eine einzelne Frage nutzen

Die fachliche Spec und nur die drei ausgewählten Dateien im Chat bereitstellen. Beispiel:

> Ich entwickle das Projekt selbst. Verwende für deine Antwort die beigefügte specs/project.md und ausschließlich die drei ausgewählten Regeldateien; beachte daneben bereits geltende Projektregeln. Schlage mir Tests für GET /products einschließlich einer leeren Liste vor. Antworte mit Codevorschlägen und einer kurzen Erklärung. Bearbeite keine Dateien, führe keine Befehle aus und übernimm keine weiteren Implementierungsschritte.

Oder für einen kleinen Implementierungsvorschlag:

> Zeige mir anhand meiner ausgefüllten Projekt-Spec eine mögliche Umsetzung des GET-Endpunkts für das dort gewählte Framework. Kennzeichne Annahmen. Ich übernehme und prüfe den Code selbst; führe keine Werkzeuge oder Folgeaufgaben aus.

Ohne KI liest du dieselben Dateien und setzt die Spec direkt in deiner IDE um. Mit KI prüfst und übernimmst du jeden Vorschlag selbst. Tests führst du selbst aus und vergleichst die Ergebnisse mit den Akzeptanzkriterien.

## 6. Später erweitern

Weitere Spec bewusst auswählen, Voraussetzungen prüfen, Auswahlmanifest und betroffene Anforderungen aktualisieren. Ein späterer agentischer Auftrag ist möglich, aber ein eigener Moduswechsel; die reine Dateiauswahl startet keinen Agenten.

**Siehe auch:** [[03-Projects/Examples/agentic-project]] · [[09-Maintenance/rule-sources]] · [[02-Standards/README]]
