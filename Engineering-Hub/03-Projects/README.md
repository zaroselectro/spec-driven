---
type: index
tags: [projects, index]
---

# Projekte

Jedes Softwareprojekt erhält hier seinen eigenen Ordner. Die für das Projekt ausgewählten zentralen Standards gelten in ihrem jeweiligen Geltungsbereich; **projektspezifische** Anforderungen, Entscheidungen und Aufgaben leben ausschließlich hier – nie in `02-Standards/`.

## Neuen Projektordner anlegen

```text
03-Projects/<projekt-kurzname>/
├── README.md                # Zweck, Stack, Geltungsbereich, Kontakt
├── Requirements/            # Anforderungen, Akzeptanzkriterien
├── Architecture/
│   ├── arc42/               # laufende Architekturbeschreibung
│   └── ADR/                 # Architektur-Entscheidungs-Registrierung
├── API/                     # Schnittstellenverträge (z. B. OpenAPI, Fehlerformat)
├── Tasks/                   # projektspezifische Aufgaben (gleiche Felder wie 05-Tasks/)
├── Testing/                 # Teststrategie, Testbefehle, Abdeckungsbetrachtung
└── Operations/              # Auslieferung, Konfiguration, Betriebsdokumentation
```

### Ergänzung: Specs-Basis im Projekt (`specs/`)

Projekte, die die Sammlung `specs/` als Spring-Boot-Basis übernehmen, tragen zusätzlich im **Projektverzeichnis des Codex-Projekts** (nicht zwingend hier):

```text
specs/
├── project.md               # Fachlichkeit, aktive Erweiterungen, Projektentscheidungen, Vorlagenstand
├── architecture.md          # verbindliche Basisarchitektur (Kopie aus specs/base/)
└── extensions/<id>.md       # nur aktivierte Erweiterungen (z. B. http-api, persistence-jpa, spring-security)
```

Zusätzlich wird eine projektweite `AGENTS.md` (Kopie aus `specs/base/AGENTS.md`). Erst die **Aktivierung in `../../../project.md`** macht eine Erweiterung gültig; eine vorhandene Datei aktiviert nichts (siehe [[02-Standards/README]], [[08-Codex/project-integration]]).

Vorbild: [[03-Projects/_Example-Project/README]] (Demonstrationsdaten). Vorlage für das README: [[07-Templates/project-template]].

## Regeln

- **MUSS** Projektregeln und die ausgewählten Specs nachvollziehbar dokumentieren. Für agentische Codex-Arbeit die `AGENTS.md` im Softwareprojekt als Einstieg verwenden; zusätzliche Dokumente ausdrücklich referenzieren (siehe [[08-Codex/project-integration]]). Für manuelle Entwicklung ist keine AGENTS-Datei erforderlich. Ein README allein bewirkt keine automatische Regelladung.
- **MUSS** Vor Projektarbeiten an einer Specs-Basis wird in dieser Reihenfolge gelesen: `../../../project.md` → `specs/architecture.md` → aktive Erweiterungen inkl. deren Voraussetzungen (siehe [[04-Workflows/single-agent-development]]).
- **MUSS** Der **Vorlagenstand** (Quelle der Sammlung, Commit-Hash, Übernahme-Datum, übernommene Dateien, lokale Änderungen) wird in `../../../project.md` festgehalten; ausdrückliche Benutzeranforderungen haben Vorrang vor der Specs-Basis.
- **MUSS** Architekturänderungen nur als ADR in `Architecture/ADR/`; akzeptierte ADRs gelten bis zum Ersatz.
- **MUSS** Keine erfundenen Anforderungen: was nicht vom Auftraggeber kommt, ist als Annahme zu kennzeichnen.
- **SOLLTE** Eine kurze Projekt-`README.md` beantwortet: Was ist das Projekt, welcher Stack, was gilt verbindlich, wo stehen die Aufgaben.
- **SOLLTE** Demonstrationsprojekte beginnen mit `_` (wie `_Example-Project`) und werden nie als reale Vorlage kopiert, ohne die Inhalte zu ersetzen.

## Beziehung zu den anderen Bereichen

| Inhalt | Ort |
| --- | --- |
| Projektregeln für Codex | `AGENTS.md` im Codex-Projektverzeichnis (Vorlagen: `08-Codex/templates/project.AGENTS.md.template`, `08-Codex/templates/backend.AGENTS.md.template`, `08-Codex/templates/infrastructure.AGENTS.md.template`) |
| Zentrale, projektübergreifende Regeln | [[02-Standards/README]] |
| Aufgaben mit Projektkontext | `03-Projects/<projekt>/Tasks/` |
| Aufgaben ohne Projektkontext bzw. Demo | [[05-Tasks/README]] |
| Vorlagen | [[07-Templates/README]] |

**Siehe auch:** Vault-Überblick in `README.md` · [[00-Dashboard/Home]]

## Konkrete Beispiele zum Projektstart

- [[03-Projects/Examples/README|Arbeitsweise auswählen]]
- [[03-Projects/Examples/agentic-project|A: Agentisch mit Projekt-Spec und ausgewählten Erweiterungen]]
- [[03-Projects/Examples/selected-specs-project|B: Selbst entwickeln, nur bestimmte Specs verwenden]]

Beide Beispiele zeigen Einrichtung, Dateiauswahl, fachliche Kriterien und optionale KI-Prompts. Sie erzeugen selbst keine Anwendung.
