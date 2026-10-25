---
type: workflow
mode: multi-agent
activation: explizit
tags: [workflow, multi-agent, optional]
---

# Multi-Agent-Entwicklung (optional)

> [!warning] Nie automatisch
> Dieser Ablauf startet **nur** auf ausdrücklichen Auftrag, z. B. „Aktiviere den Multi-Agenten-Workflow für diese Aufgabe.“ Jeder andere Auftrag läuft als [[04-Workflows/single-agent-development]]. Eine Rollenbeschreibung löst von selbst nichts aus.

> [!info] Was hier fehlt: die Laufzeit
> Vollständig **dokumentiert** sind: Aufgaben-IDs, Verantwortlichkeiten, Abhängigkeiten, Status, Übergaben, Reviews, Konflikte, Branches/Worktrees, Freigabepunkte. **Nicht implementiert** ist ein Programm, das Agenten startet, Fortschritt überwacht und Zustände erzwingt. Bis dahin laufen die Schritte als nacheinander ausgeführte Codex-Sitzungen (bzw. Aufgabenpakete), die ein Mensch anstößt und deren Ergebnisse er zusammenführt. Details: [[09-Maintenance/creation-report]].

## Aktivierung

1. Auftraggeber aktiviert ausdrücklich (im Prompt oder in der Aufgabe: `mode: multi-agent`).
2. [[01-Agents/orchestrator]] übernimmt Koordination: Zerlegung, Zuteilung, Status, Zusammenführung.
3. Jeder Schritt läuft in **einer** Sitzung mit **einer** Rollenperspektive; die Rolle startet nie selbst weitere Agenten.
4. Endet mit zusammengeführtem Ergebnis und benannten Restpunkten.

## Ablauf am Beispiel eines Produktivfeatures

| Schritt | Rolle | Ergebnis | Voraussetzung |
| --- | --- | --- | --- |
| 1 | [[01-Agents/orchestrator]] | Analyse, Aufgabenpakete mit IDs, Abhängigkeiten | Nutzerauftrag |
| 2 | [[01-Agents/software-architect]] | Architekturvorschlag als ADR (`vorgeschlagen`) | Schritt 1 |
| 3 | *Freigabepunkt* | ADR akzeptiert | Auftraggeber |
| 4 | [[01-Agents/backend-developer]] | Backend-Umsetzung + Tests | Schritt 3 |
| 5 | [[01-Agents/frontend-developer]] | Oberfläche, sofern im Umfang | Schnittstellen aus Schritt 4 |
| 6 | [[01-Agents/qa-engineer]] | Testergebnisse, Befunde | Schritt 4 (5) |
| 7 | [[01-Agents/security-reviewer]] | Sicherheitsbefunde, Freigabeempfehlung | Schritt 4 (5) |
| 8 | [[01-Agents/devops-engineer]] | Deployment-Vorbereitung | Schritt 6, 7 |
| 9 | *Freigabepunkt* | Zusammenführung, Status, Restpunkte | Schritte 6–8 |

Schritte 5 und 8 entfallen, wenn nicht betroffen. Reihenfolge von 6 und 7 ist frei wählbar; beide sind Voraussetzung für 9.

## Aufgaben-IDs und Status

- **Format:** `<KÜRZEL>-<TYP>-<NUMMER>`, z. B. `DEMO-BE-001`. Kürzel: `DEMO` = Demonstrationsprojekt (im echten Projekt das Projektkürzel). Typen: `ARCH`, `BE`, `FE`, `QA`, `SEC`, `OPS`.
- Jede Aufgabe liegt als Datei in `05-Tasks/` oder `03-Projects/<projekt>/Tasks/` mit den Feldern aus [[07-Templates/task-template]].
- **Statusablauf:** `backlog → in-progress → review → done` (siehe [[05-Tasks/README]]).
- `depends_on` enthält dieIDs der Voraufgaben; eine Aufgabe startet erst, wenn alle Voraufgaben `done` sind.
- `assigned_to` enthält **genau eine** Rolle; `review_by` nennt die prüfende Rolle (in der Regel eine andere).

## Übergaben, Reviews, Ergebnisse

- Übergabeformat: [[04-Workflows/agent-handover]] (7 Pflichtangaben).
- Reviews erfolgen nach [[04-Workflows/code-review]] mit Schweregraden; kritische/hohebefunde blockieren.
- Review-Ergebnisse werden in der Aufgabe festgehalten (Feld `review_by`, Abschnitt „Ergebnis“), nicht nur im Fließtext.

## Konfliktbehandlung

- Zuständigkeits-, Vorgaben- und Änderungskonflikte: siehe Tabelle in [[04-Workflows/agent-handover]] – benennen, eskalieren, nicht still auflösen.
- Widerrufene oder widersprüchliche Anforderungen: Umsetzung stoppen, Entscheidung beim Auftraggeber.
- Mehrere Aufgaben auf dieselbe Datei: eine Aufgabe pausieren, Reihenfolge durch Orchestrator festlegen.

## Git: Branches und Worktrees

- **SOLLTE** Pro Teilaufgabe eigener Branch bzw. eigener Worktree; Name enthält Aufgaben-ID (Raster: Projektentscheidung).
- **MUSS** Kein Überschreiben fremder Änderungen; Konflikte werden gemeldet und in Zusammenführung mit Begründung gelöst.
- **MUSS** Zusammenführen erst nach bestandenem Review und erfüllten Freigabepunkten.
- **SOLLTE** Hauptlinie bleibt lauffähig; längere Arbeit erfolgt in der Änderungseinheit, nicht auf der Hauptlinie.

## Freigabepunkte für kritische Änderungen

Vor dem Zusammenführen oder vor einer Auslieferung ist freizugeben, wenn mindestens eines zutrifft:

1. **Architektur**: akzeptiertes ADR wird neu entschieden oder gebrochen → Auftraggeber.
2. **Sicherheit**: Authentifizierung/Autorisierung, Secrets, öffentliche Schnittstelle, Datenexport → [[01-Agents/security-reviewer]] + Auftraggeber.
3. **Daten in Betrieb**: Datenbank-Schema- oder Nachrichtenformat-Änderung mit Bestandsnutzern → Rückwärtskompatibilität belegt.
4. **Auslieferung produktiv**: Deployment-Vorbereitung durch [[01-Agents/devops-engineer]], Freigabe durch den Menschen.
5. **Zentrale Standards**: Regel in `02-Standards/` soll geändert werden → Dokumentation im betroffenen ADR bzw. in der Standarddatei.

## Nicht Teil dieser Datei

- Keine Beschreibung einer Orchestrierungssoftware, die Agenten selbst startet.
- Keine automatische Statusverwaltung – Status wird von den Beteiligten gepflegt.
- Keine Berechtigungserteilung: Rollen erhalten keine Rechte über das, was der Mensch der Sitzung ohnehin gibt.

**Siehe auch:** [[04-Workflows/single-agent-development]] · [[04-Workflows/agent-handover]] · [[04-Workflows/feature-development]] · [[01-Agents/orchestrator]] · [[05-Tasks/README]]
