---
type: knowledge
topic: java
tags: [wissen, java]
---

# Wissen: Java

Erfahrungs- und Hintergrundwissen zu Java 21. **Verbindlich** ist ausschließlich [[02-Standards/Java/java-standards]].

## Sprachstand, der sich im Alltag auszahlt

- **`record`** für Datenträger: weniger Code, automatische Unveränderlichkeit, sinnvolle `equals`/`hashCode`/`toString`.
- **Sealed Types** für geschlossene Varianten (z. B. Ergebnisarten) – `switch` über sie kann erschöpfend geprüft werden.
- **Pattern Matching** für `instanceof` und `switch`: entfernt manuelle Abfragen und macht Varianten lesbar.
- **`Optional`** an API-Grenzen, um „kein Wert“ auszudrücken; als Feld oder Parameter ist es ein Zeichen für ungeklärte Modellierung.
- **Virtual Threads** können gebundene I/O-Arbeit entlasten; ob und wo sie eingesetzt werden, ist eine Projektentscheidung – kein Selbstzweck.

## Häufige Fallstricke

| Stolperstein | Wirkung | Alternative |
| --- | --- | --- |
| `catch (Exception e) {}` | Fehler verschwinden, Diagnose unmöglich | behandeln oder mit Ursache weiterreichen |
| Statische, veränderbare Zustände | Tests beeinflussen sich gegenseitig | Instanzen mit injizierten Abhängigkeiten |
| Logik in Controllern/Utils-Klassen | Geschäftsregeln schwer auffindbar | eigene Schicht, benannte Objekte |
| Streams mit Nebenwirkungen | unübersichtliche Ausführung | Schleife, wenn Wirkung im Zentrum steht |
| Datumsrechnung ohne feste Zone | „Morgen“ verschiebt sich je nach Server | feste Zeitzonen, UTC als Referenz |

## Testbarkeit

Was Zeit, Netz oder Zufall direkt verwendet, lässt sich schwer prüfen. Kurze Grenzen einführen (Übergabe als Parameter, kleine Schnittstelle) und in Tests kontrollieren – Details: [[02-Standards/General/architecture-principles]] und [[02-Standards/Testing/testing-standards]].

## Einstieg in dieses Vault

Standard: [[02-Standards/Java/java-standards]] · Build: [[02-Standards/Java/maven-standards]] · Framework: [[02-Standards/Java/spring-boot-standards]] · Rolle: [[01-Agents/backend-developer]]

**Siehe auch:** [[06-Knowledge/README]]
