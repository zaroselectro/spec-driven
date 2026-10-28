---
type: workflow
tags: [workflow, review, qualitaet]
---

# Workflow: Code-Review

Gilt für jede Änderung, die über eine offensichtliche Korrektur hinausgeht. Ziel: Befunde vor dem Zusammenführen, nicht danach. Der Ablauf läuft im **Single-Agent-Modus** genauso wie im Team – die prüfende Perspektive wird einfach mitgesetzt.

Für einen Review-Auftrag steht das Agent-Paket `hub-code-review` zur Verfügung. Aufruf und Projektanbindung: [[10-Skills/agent-skills]].

## Voraussetzungen

- Änderungsumfang steht (Diff, Aufgabe, Akzeptanzkriterien).
- Zugehörige Aufgabe in `05-Tasks/` im Status `review`.
- Die vom Diff berührten Standards wurden gelesen – nur diese.

## Prüfung

1. **Absicht verstehen.** Welches Akzeptanzkriterium soll die Änderung erfüllen? Ohne diese Antwort kein Stil-Kommentar.
2. **Korrektheit.** Stimmen Hauptpfad und Fehlerpfade? Gibt es Randfälle (leere Mengen, Null, Doppelte, Zeit)? (Standards: [[02-Standards/General/clean-code]], Fachstandards.)
3. **Verbindliche Regeln.** Schichtenordnung, Fehlerbehandlung, Protokollierung, keine Secrets, Idempotenz bei Ereignissen, Testabdeckung der Kriterien.
4. **Tests.** Laufen sie? Prüfen sie Verhalten statt Implementierung? Sind Fehlerpfade abgedeckt? (Siehe [[02-Standards/Testing/testing-standards]].)
5. **Sicherheit** bei den einschlägigen Fällen: neue/veränderte Schnittstelle, Authentifizierung/Autorisierung, Datenexport, Abhängigkeiten, Konfiguration. Umfang nach [[02-Standards/General/security-principles]]; bei sicherheitsrelevanten Änderungen zieht [[01-Agents/security-reviewer]] zwingend hinzu.
6. **Betrieb** bei Konfigurations-, Bild- oder Manifeständerungen: Ressourcen, Prüfungen, Secrets-Quelle, Rollback (siehe `02-Standards/Infrastructure/`).

## Ergebnis

Befunde über [[07-Templates/review-template]]: Schweregrad, Fundstelle, Feststellung, Empfehlung. Sortiert nach Schwere. Jeder Befund ist konkret und nachvollziehbar – keine Geschmacksurteile als Blocker.

## Umgang mit Befunden

| Schwere | Wirkung |
| --- | --- |
| kritisch (Sicherheit, Datenverlust, Verstoß gegen **MUSS**) | Blockiert; Behebung durch die implementierende Rolle, danach Nachprüfung |
| hoch (falsches Verhalten, fehlende Tests für Kriterium) | Blockiert die Freigabe; Behebung erforderlich |
| mittel (Wartbarkeit, unklarer Name, doppelter Pfad) | Behebung vor oder unmittelbar nach dem Zusammenführen, als Aufgabe sichtbar |
| niedrig (Verbesserungsvorschlag) | Optional, dokumentiert |

- **MUSS** Keine „grüne“ Freigabe mit offenen kritischen oder hohen Befunden.
- **MUSS** Der Prüfer prüft nicht seine eigene Änderung allein (im Team: andere Rolle; im Einzelmodus: getrennte Prüfperspektive im Prompt und ehrlicher Ausweis dessen).
- **SOLLTE** Zustimmung und Befund werden getrennt ausgewiesen („freigabefähig mit Auflagen“ ist ein valider Zustand).

## Statuswechsel

Nach bestandener Prüfung: Aufgabe von `review` nach `done` (bzw. zurück zu `in-progress` bei Blockern) – siehe [[05-Tasks/README]].

**Siehe auch:** [[04-Workflows/feature-development]] · [[04-Workflows/agent-handover]] · [[01-Agents/qa-engineer]] · [[01-Agents/security-reviewer]]
