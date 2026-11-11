# Spring Boot 3.x

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Kompatible Spring-Boot-3.x-Version im Projekt festlegen. Constructor Injection nutzen. Konfiguration externalisieren und relevante Properties validieren. Transaktionsgrenzen in der Anwendungs-/Geschäftslogik setzen. REST-DTOs von Persistenzmodellen trennen.

Für deployte Services gelten die [[02-Standards/General/cloud-native-standards|Cloud-native Engineering Standards]] einschließlich Timeout, definierter Retry Policy, Health/Readiness, Observability und Graceful Shutdown. Technische Endpunkte bedarfsgerecht bereitstellen und passend absichern.

## Empfehlungen (SOLLTE)
Zentrale Fehlerabbildung mit stabilen Fehlercodes; strukturierte Logs mit Korrelationskennung.

## Projektentscheidungen
Konkrete Version, Blocking/Reactive, Transaktionsisolation und exposed Actuator-Endpunkte bestimmen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
