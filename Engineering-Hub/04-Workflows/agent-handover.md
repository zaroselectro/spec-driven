---
type: workflow
tags: [workflow, uebergabe, koordination]
---

# Workflow: Agentenübergabe

Formate und Regeln für die Übergabe einer Teilaufgabe von einer Rolle an die nächste. Gilt im Einzelmodus genauso für die Übergabe zwischen Sitzungen – die Struktur ist dieselbe, nur wer sie empfängt, unterscheidet sich.

## Übergabeformat

Jede Übergabe enthält – kurz und vollständig:

1. **Aufgaben-ID** und Titel (z. B. `DEMO-BE-001`), Status der Aufgabe.
2. **Was wurde getan** – Änderungsumfang in wenigen Zeilen, betroffene Pfade/Module.
3. **Artefakte** – Dateien, Dokumente, Entwürfe, ADR-Verweise.
4. **Prüfungen** – welcher Befehl wurde ausgeführt, welches Ergebnis (und was **nicht** geprüft wurde).
5. **Offene Punkte und Annahmen** – getrennt, mit Frage an wen.
6. **Nächste Verantwortung** – Rolle, erwartetes Ergebnis, Abhängigkeiten.
7. **Einschränkungen** – was ausdrücklich nicht angefasst wurde und warum.

Vorlage: [[07-Templates/review-template]] (Review) bzw. die Felder in [[07-Templates/task-template]] (Aufgabe).

## Regeln

- **MUSS** Eine Übergabe nennt immer eine konkrete empfangende Rolle und eine Aufgaben-ID. „Irgendwer schaut drauf“ ist keine Übergabe.
- **MUSS** Annahmen sind als Annahmen markiert; offene Fragen werden nicht still beantwortet.
- **MUSS** Prüfergebnisse werden mit Befehl und Ergebnis belegt, nicht behauptet.
- **MUSS** Der Empfangende liest die Aufgabe und die zugehörigen Standards **selbst** – er übernimmt nicht blind, was behauptet wird (Verantwortung bleibt beim Empfangenden).
- **SOLLTE** Übergaben sind so kurz wie möglich und so vollständig wie nötig; Verweise ersetzen Abschriften (kein Kopieren großer Texte, sondern Links auf Dateien).
- **SOLLTE** Bei Status `review` ist ein Review-Ergebnis mit Schweregraden erforderlich (siehe [[04-Workflows/code-review]]).

## Konflikte

Konflikte werden benannt, nicht still aufgelöst:

| Art des Konflikts | Vorgehen |
| --- | --- |
| Widerspruch zwischen Auftrag und ADR | Umsetzung stoppen, Konflikt an [[01-Agents/orchestrator]] bzw. Auftraggeber, Entscheidung als neue/ergänzte ADR |
| Widerspruch zwischen zwei **MUSS**-Regeln | Festhalten, welche beiden Regeln kollidieren und in welchem Konkreten; Entscheidung beim Auftraggeber, Dokumentation im ADR |
| Doppelte Arbeit / gleiche Datei in zwei Aufgaben | Zuordnung klären, eine Aufgabe pausieren; niemals fremde Änderungen überschreiben |
| Merge-Konflikt im Code | Klären, wer die Änderung besitzt; Zusammenführung mit Begründung, nicht nach Zufall |
| Technische Unsicherheit | Offener Punkt mit Kennzeichnung „nicht verifiziert“ und vorgeschlagener Prüfung |

## Git: Branches und Worktrees

- **SOLLTE** Arbeit in einer eigenen Änderungseinheit pro Teilaufgabe (Branch bzw. Worktree), damit Übergaben und Reviews sauber trennbar sind.
- **MUSS** Keine fremden, unveränderten Änderungen überschreiben; vorhandene Arbeit wird respektiert.
- **SOLLTE** Änderungsnachrichten nennen Aufgaben-ID und Inhalt.
- **Projektentscheidung:** Branch-Namensraster, Schutz der Hauptlinie, Worktree-Nutzung. (Ob und wie Git tatsächlich bedient wird, richtet sich nach dem Projekt und den vorhandenen Berechtigungen.)

## Freigabepunkte

Kritische Änderungen brauchen vor dem Zusammenführen einen benannten Freigabepunkt:

- Akzeptiertes ADR berührt (Architektur)
- Sicherheitsrelevanz (Authentifizierung/Autorisierung, Secrets, öffentliche Schnittstelle) – [[01-Agents/security-reviewer]]
- Datenbank- oder Nachrichtenformat in Betrieb (Rückwärtskompatibilität)
- Deployment in eine produktive Umgebung – [[01-Agents/devops-engineer]] plus menschliche Freigabe

**Siehe auch:** [[04-Workflows/multi-agent-development]] · [[04-Workflows/single-agent-development]] · [[01-Agents/orchestrator]]
