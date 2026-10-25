---
type: placeholder
tags: [projekt, api]
---

# API – _Example-Project

**Bewusst vorgesehener Bereich für spätere Inhalte.** Hier liegen die Schnittstellenverträge des Projekts, sobald sie feststehen.

## Was hierher gehört

- Schnittstellenbeschreibung (z. B. OpenAPI-Datei oder eine kompakte Ressourcenliste).
- Fehlerformat und Beispiele für Fehlerantworten.
- Paginierungs- und Versionskonvention des Projekts.
- Verweise auf die zugehörigen ADRs und auf [[02-Standards/API/rest-standards]].

## Regeln

- **MUSS** Der Vertrag ist die Quelle für Backend **und** Frontend; Abweichungen in der Implementierung werden im Vertrag nachgezogen, nicht still geduldet.
- **MUSS** Keine echten Daten, Zugangsdaten oder personenbezogenen Beispieldaten in Vertragsdateien.
- **SOLLTE** Rückwärtskompatible Änderungen zuerst; Brüche nur mit neuer Version (siehe [[02-Standards/API/rest-standards]]).
- **SOLLTE** Beispiele als Demonstrationsdaten kennzeichnen.

## Zuständigkeit

Erstellung und Pflege: [[01-Agents/software-architect]] bzw. [[01-Agents/backend-developer]]; Nutzung: [[01-Agents/frontend-developer]].

**Siehe auch:** [[03-Projects/_Example-Project/README]]
