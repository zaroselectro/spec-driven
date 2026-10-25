---
type: workflow
tags: [workflow, architektur]
---

# Workflow: Architekturarbeit

Gilt, wenn Struktur, Schnittstellenkonzept oder technische Alternative **entschieden** werden soll. Keine Pipeline, kein automatischer Start – der Ablauf wird manuell durchlaufen.

## Voraussetzungen

- Anforderungen bzw. Aufgabe mit Umfangsgrenze (was zu entscheiden ist, was nicht).
- Geltende Standards und vorhandene ADRs des Projekts gelesen (nur die einschlägigen).
- Rolle gesetzt: [[01-Agents/software-architect]] (in der Codex-Sitzung als Perspektive).

## Ablauf

1. **Eingrenzen.** Problem, Nicht-Ziele und offene Fragen aufschreiben. Fehlende Anforderungen als Annahme kennzeichnen, nicht erfinden.
2. **Bestand aufnehmen.** Betroffene arc42-Abschnitte und bestehende ADRs prüfen: Was ist bereits entschieden? Entscheidungen gelten weiter, bis ein neues ADR sie ersetzt.
3. **Alternativen bewerten.** Mindestens zwei ernstzunehmende Optionen, wenn die Entscheidung Wirkung hat; Bewertungskriterien vor dem Vergleich festlegen (z. B. Betriebsaufwand, Lernkurve, Passung zur bestehenden Architektur, Risiko).
4. **Empfehlung formulieren.** Mit Begründung und den Konsequenzen, die mit der Wahl kommen.
5. **ADR entwerfen.** [[07-Templates/adr-template]], Status `vorgeschlagen`, Ablage: `03-Projects/<projekt>/Architecture/ADR/`.
6. **Prüfung einholen.** Sicherheitsrelevante Teile durch [[01-Agents/security-reviewer]], Betriebsteile durch [[01-Agents/devops-engineer]], Machbarkeit aus Implementierung und QA. Ergebnisse ergänzen, solange der Status `vorgeschlagen` ist.
7. **Freigabe.** Der Auftraggeber akzeptiert; Status wird auf `akzeptiert` gesetzt. **Erst danach** wird umgesetzt.
8. **Nachziehen.** arc42-Abschnitte und betroffene Standards aktualisieren; betroffene Aufgaben anlegen (abhängig von der Architekturaufgabe).

## Ergebnis

- ADR mit Problem, Optionen, Kriterien, Ergebnis, Konsequenzen.
- Aktualisierte arc42-Beschreibung bzw. konkrete Änderungsliste.
- Liste der Annahmen und offenen Punkte.

## Regeln

- **MUSS**: Keine Umsetzung vor akzeptiertem ADR.
- **MUSS**: Bestehende ADRs werden nicht still überschrieben.
- **MUSS**: Zentrale Standards nur mit dokumentierter Ausnahme verlassen.
- **SOLLTE**: Bei einfacher, im Projekt bereits geklarer Fragestellung keinen vollen Architekturprozess aufmachen – kurze Entscheidung festhalten und weiter.

**Siehe auch:** [[04-Workflows/single-agent-development]] · [[04-Workflows/multi-agent-development]] · [[02-Standards/General/architecture-principles]] · [[01-Agents/software-architect]]
