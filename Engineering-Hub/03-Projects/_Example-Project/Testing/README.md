---
type: placeholder
tags: [projekt, testing]
---

# Testing – _Example-Project

**Bewusst vorgesehener Bereich für spätere Inhalte.** Hier entstehen Teststrategie und Testbefehle des Projekts.

## Was hierher gehört

- **Teststrategie**: welche Ebenen einsetzt werden (Unit, Integration, End-to-End) und warum – angelehnt an [[02-Standards/Testing/testing-standards]].
- **Testbefehle**: die konkreten Kommandos, die vor der Abgabe laufen (z. B. `mvn test`, `mvn verify`, Frontend-Testbefehl) – sie gehören später in die Projekt-`AGENTS.md`.
- **Testumgebung**: was die Integrationstests bereitstellen (Datenbank, Broker), welche Werkzeuge gewählt wurden (Projektentscheidung).
- **Abdeckungsbetrachtung**: welche fachlichen Fälle abgedeckt sind, welche bewusst nicht.
- **Bekannte Instabilitäten**: flaky Tests mit Aufgabe statt Duldung.

## Regeln

- **MUSS** Akzeptanzkriterien sind zuordenbar zu Tests oder zu dokumentierten Ausnahmen.
- **MUSS** Keine echten personenbezogenen Daten in Testfixtures.
- **SOLLTE** Testbefehle sind identisch lokal und in der Pipeline.
- **SOLLTE** Langsame Tests sind gekennzeichnet, damit sie gezielt ausgewählt werden können.

## Zuständigkeit

Strategie und Umsetzung: [[01-Agents/qa-engineer]]; Tests zur eigenen Änderung liefern die implementierenden Rollen mit.

**Siehe auch:** [[03-Projects/_Example-Project/README]] · [[04-Workflows/code-review]]
