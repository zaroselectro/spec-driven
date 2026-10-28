---
name: implement-from-spec
description: Implementiere eine benannte Spezifikation im Zielprojekt von der Anforderungsanalyse über einen Implementierungsplan bis zu Code, Tests und Soll-Ist-Abgleich. Nutze den Skill bei einem Auftrag zur Umsetzung einer Spec, nicht für reine Spezifikationsreviews oder allgemeine Codeänderungen ohne Spec-Bezug.
---

# Implement from Spec

Setze die vom Nutzer benannte Spezifikation im angegebenen Zielprojekt um. Benötigt werden die Spec oder ihr zugänglicher Inhalt sowie das Zielprojekt. Leite beides aus dem Auftrag und vorhandenen Projektkontext ab; frage nur nach, wenn die Zuordnung nicht eindeutig ist. Beispiele und inaktive Erweiterungen sind keine zusätzlichen Anforderungen.

## 1. Spezifikation lesen — Anforderungen und Akzeptanzkriterien

- Lies die beauftragte Spec und die für ihr Verständnis nötigen Referenzen.
- Ermittle funktionale Anforderungen, ausdrücklich genannte Qualitätsanforderungen, Akzeptanzkriterien und Abgrenzungen. Behalte vorhandene Anforderungs-IDs bei; nutze sonst kurze lokale Kennungen für den späteren Abgleich.
- Prüfe vorhandenen Code und Tests darauf, welche Anforderungen bereits erfüllt sind.
- Kennzeichne fehlende oder widersprüchliche Kriterien. Leite prüfbare Kriterien aus eindeutig beschriebenem Verhalten ab und weise diese Ableitung aus. Erfinde keine Fachentscheidungen; kläre blockierende Unklarheiten und arbeite an unabhängigen Teilen weiter.

## 2. Standards ermitteln — Aktive Projektregeln und ADRs

- Lies die geltenden `AGENTS.md`-Dateien und nur die für die betroffenen Komponenten relevanten Projektstandards und akzeptierten ADRs.
- Ermittle bei modularen Specs die tatsächlich aktivierte Basis und Erweiterungen anhand der Projektkonfiguration. Die bloße Existenz einer Spec-Datei aktiviert sie nicht.
- Berücksichtige vorhandene Architektur, Schnittstellen, Konventionen und vorgeschriebene Prüfbefehle. Nutze im Engineering-Hub die [Regelquellen](../../../09-Maintenance/rule-sources.md) zur Auswahl; technische Regeln werden dort und in den referenzierten Standards gepflegt.
- Benenne echte Konflikte zwischen Anforderungen und geltenden Entscheidungen. Kläre nur die betroffene Entscheidung vor deren Implementierung; bereits bestätigte Entscheidungen gelten weiter.

## 3. Implementierungsplan erstellen — Abhängigkeiten und betroffene Module

- Erstelle einen dem Umfang angemessenen Plan mit betroffenen Modulen, Schnittstellen, nötigen Änderungen und ihrer Reihenfolge. Berücksichtige technische Abhängigkeiten und gegebenenfalls Datenmigrationen.
- Ordne den Anforderungen die geplanten Implementierungsschritte und Nachweise zu. Unterscheide bereits erfüllte Anforderungen von noch nötigen Änderungen.
- Nutze bei kleinen Aufgaben einen kurzen Plan im Chat; lege eine separate Plan-Datei nur an, wenn Auftrag oder Projektablauf sie verlangen.
- Fahre innerhalb des erteilten Auftrags mit der Umsetzung fort. Der Plan allein erzeugt keinen zusätzlichen Freigabeschritt. Arbeite als einzelner Agent, sofern kein Teamauftrag vorliegt.

## 4. Implementierung durchführen — Code und Tests

- Implementiere die noch fehlenden Anforderungen entlang des Plans und der bestehenden Projektstruktur. Erhalte fremde Änderungen und vermeide unabhängige Refactorings oder zusätzliche Features.
- Ergänze oder aktualisiere aussagekräftige Tests für das geforderte Verhalten, einschließlich relevanter Fehler- und Randfälle. Nutze bestehende Teststrukturen; vermeide Tests, die lediglich die Implementierung nachbilden.
- Aktualisiere betroffene Verträge, Konfiguration oder Dokumentation, soweit dies für die Umsetzung nötig ist.
- Passe den Plan bei neuen Erkenntnissen an und mache wesentliche Auswirkungen auf Umfang oder Abhängigkeiten sichtbar.

## 5. Validieren — Tests, Review und Soll-Ist-Abgleich

- Führe die relevanten vorhandenen Tests und vorgeschriebenen Projektprüfungen aus. Behebe durch die Änderung verursachte Fehler und wiederhole die betroffenen Prüfungen. Unterscheide bestehende Fehler von neu verursachten.
- Prüfe den finalen Diff auf Korrektheit, Regressionen und Einhaltung der ermittelten Regeln. Nutze im Hub den [Review-Workflow](../../../04-Workflows/code-review.md) für die Prüfkriterien. Weise das Review eigener Änderungen als Selbstprüfung aus; es belegt kein unabhängiges Review.
- Vergleiche jede Anforderung bzw. jedes Akzeptanzkriterium mit dem tatsächlichen Ergebnis. Nenne Umsetzung und belastbaren Nachweis, etwa Testfall, ausgeführten Befehl oder konkrete Codefundstelle. Eine Codefundstelle allein belegt keinen erfolgreichen Laufzeittest.
- Kennzeichne Kriterien als erfüllt, teilweise erfüllt, offen oder nicht geprüft. Melde fehlende Werkzeuge, nicht ausgeführte Prüfungen und verbleibende Abweichungen ausdrücklich. Behaupte bei offenen oder ungeprüften Kriterien keine vollständige Erfüllung.

## Ergebnisbericht

Berichte knapp über Änderungen und betroffene Module, ausgeführte Prüfungen samt Ergebnis sowie offene Punkte. Ergänze den Soll-Ist-Abgleich in einer dem Umfang angemessenen Tabelle:

| Anforderung / Kriterium | Umsetzung | Nachweis | Status / Abweichung |
| --- | --- | --- | --- |

Benenne den Stand der umgesetzten Spec, soweit er aus dem Auftrag oder Projekt eindeutig feststellbar ist. Der Skill erweitert den Auftrag nicht um Deployment, Veröffentlichung oder Zusammenführung.
