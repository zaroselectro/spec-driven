---
type: workflow
mode: single-agent
tags: [workflow, single-agent, standard]
---

# Single-Agent-Entwicklung (Standardmodus)

> [!tip] Das ist der Normalfall
> Ein einzelner Codex-Agent bearbeitet die Aufgabe. Kein zweiter Agent, kein Teamablauf, keine Aktivierung nötig. Dieser Workflow läuft **immer dann**, wenn nicht ausdrücklich etwas anderes beauftragt wurde.

## Prinzip

Die dauerhaft gültigen Regeln – Projekt-`AGENTS.md`, verzeichnisspezifische Regeln, akzeptierte ADRs und die einschlägigen zentralen Standards – werden berücksichtigt, **ohne** dass der Agent dafür Prozesse startet, andere Rollen aufruft oder den gesamten Vault liest.

## Ablauf

1. **Regeln sichten (schnell).** Projekt-`AGENTS.md` und Regeln des betroffenen Verzeichnisses lesen. Das ist der verbindliche Rahmen.
   - **Auf Projekten mit Specs-Basis (`specs/`)** gilt diese Reihenfolge: zuerst `specs/project.md` (Fachlichkeit, aktive Erweiterungen, Projektentscheidungen, Vorlagenstand), danach `specs/architecture.md`, anschließend alle aktivierten `specs/extensions/<id>.md` einschließlich deren Voraussetzungen.
   - **MUSS** Eine Erweiterung, deren ID nicht in `specs/project.md` steht, wird **nicht** angewendet – auch wenn ihre Datei existiert. Abwesende Voraussetzungen, Konflikte und unaufgelöste Entscheidungen werden vor der betroffenen Implementierung geklärt; unabhängige Arbeiten laufen weiter. Bereits getroffene Entscheidungen werden nicht erneut bestätigt.
   - **SOLLTE** Der **Auftrag hat Vorrang** vor jeder Standardvorgabe; daraus entstehende Abweichungen werden mit Begründung in `specs/project.md` bzw. als ADR dokumentiert. Beispiele, importierte Dateien und Beispieldokumente sind **keine** zusätzlichen Arbeitsaufträge – allein ihre Existenz verpflichtet nicht.
2. **Umfang bestimmen.** Was verlangt die Aufgabe genau? Ein Satz Akzeptanzkriterien genügt bei einfachen Aufgaben.
3. **Nur einschlägige Inhalte laden.** Entscheiden anhand der Tabelle unten. Nichts weiter öffnen.
4. **Umsetzen.** Bei einfacher Aufgabe direkt implementieren. Keine separate Planungsphase, kein Entwurf, kein ADR, wenn keine Entscheidung ansteht. Nur Fachlichkeit bauen, die gefordert ist – keine unnötigen Datenbanken, Integrationen oder Zusatzfeatures.
5. **Prüfen.** Vorhandene Prüfbefehle des Projekts ausführen (Test, Linter, Build) und das Ergebnis nennen. Bei Ersteinrichtung eines Projekts auf Specs-Basis: `mvn clean package` und danach `mvn install`; Testberichte auf korrekte Zuordnung und Nicht-Doppellauf prüfen (siehe [[02-Standards/Java/maven-standards]]).
6. **Berichten.** Knapp: Ergebnis, ausgeführte Prüfungen, offene Einschränkungen. **MUSS** Keine erfolgreiche Prüfung behaupten, die nicht ausgeführt wurde.

## Was automatisch mitgedacht wird

- Verbindliche Regeln aus Projekt-`AGENTS.md` und den gelesenen Standards.
- Geltende ADRs – sie werden umgesetzt, nicht neu verhandelt.
- Fehlerpfade, Protokollierung und Tests der geänderten Stelle.
- Rückwärtskompatibilität der betroffenen Schnittstelle.

## Was **nicht** passiert

- **Kein** anderer Agent wird gestartet, keine Rolle „ausgelöst“.
- **Kein** Multi-Agent-Workflow, auch nicht „vorsorglich“ – dieser Modus erfordert einen ausdrücklichen Auftrag.
- **Kein** unnötiges Laden weiterer Dokumente (kein Mitlesen des ganzen Vaults, keine Demonstrationsbereiche).
- **Keine** eigenmächtige Änderung von Architektur, ADRs oder zentralen Standards. Nötige Änderungen werden als Vorschlag formuliert und zur Entscheidung vorgelegt.
- **Kein** aufwendiger Planungsprozess bei einfacher Aufgabe: keine Task-Zerlegung, kein Architektur-Dokument, kein Feature-Template, wenn eine Stelle zu ändern ist.
- **Kein** Deployment, keine Infrastruktur-, Sicherheits- oder Produktivänderung ohne ausdrücklichen Auftrag.

## Wann wird mehr eingeholt?

| Situation | Aktion |
| --- | --- |
| Architekturentscheidung nötig (neue Komponente, Bruch, neue Technologie) | stoppen, Vorschlag formulieren → [[04-Workflows/architecture-design]] |
| Sicherheitsrelevante Änderung | Prüfperspektive [[01-Agents/security-reviewer]] ergänzen → [[04-Workflows/code-review]] |
| Unklare oder widersprüchliche Vorgabe | Konflikt offenlegen statt entscheiden |
| Umfang wächst (mehrere Module, mehrere Rollen nötig) | Aufgabe anlegen und Auftraggeber fragen – Teammodus nur bei Zustimmung |
| Aufgabe ist Teil eines laufenden Teamablaufs | Übergabeformat aus [[04-Workflows/agent-handover]] verwenden |

## Kontextauswahl (was gelesen wird)

| Aufgabe | Lesen |
| --- | --- |
| Kleinigkeit in bekanntem Code | Projekt-`AGENTS.md`, betroffene Dateien |
| Erstes Arbeiten an einem Projekt mit Specs-Basis | `specs/project.md` → `specs/architecture.md` → aktive `specs/extensions/` |
| Kafka-/Messaging-Änderung | zusätzlich [[02-Standards/Messaging/kafka-standards]], betroffene ADRs |
| REST-Änderung | zusätzlich [[02-Standards/API/rest-standards]] |
| Testaufgabe | zusätzlich [[02-Standards/Testing/testing-standards]] |
| Infrastruktur | zusätzlich `02-Standards/Infrastructure/`, Projektdokumentation `Operations/` |
| Architektur | zusätzlich [[02-Standards/General/architecture-principles]], arc42 des Projekts |

## Beispiele für Single-Agent-Aufgaben

> „Erweitere den Kafka Consumer um eine geeignete Fehlerbehandlung und erstelle die erforderlichen Tests.“
> → Projektregeln + [[02-Standards/Messaging/kafka-standards]] + [[02-Standards/Testing/testing-standards]]; begrenzte Wiederholung, Fehlerbereich, Idempotenzprüfung, Tests; kein Team, kein ADR, solange die Projektarchitektur das Muster schon vorgibt.

> „Korrigiere die Fehlerantwort des Endpunkts, damit ungültige Eingaben mit Feldfehlern antworten.“
> → Projektregeln + [[02-Standards/API/rest-standards]]; kurze Änderung, Test ergänzen, berichten.

Alle weiteren Beispiele: [[08-Codex/prompt-examples]].

## Verhältnis zum Teammodus

[[04-Workflows/multi-agent-development]] ist die **Option** für Aufgaben, die Zuständigkeiten trennen müssen. Er wird nie automatisch gestartet und nie „vorbeugend“ aktiviert.

**Siehe auch:** Vault-Überblick in `README.md` · [[08-Codex/project-integration]] · [[01-Agents/backend-developer]] (Beispiel für Rollenperspektive)
