# Engineering Hub

Konsolidierte Wissensbasis aus `foo`, `mimo` und `software-engineering`, erstellt am 2026-10-25. Dieser Ordner ist die neue zusammengeführte Arbeitsbasis; die drei Ursprungsordner sind unverändert erhalten. Rollen beschreiben Perspektiven und starten keine Agenten.

## Öffnen und Einstieg
In Obsidian „Ordner als Vault öffnen“ wählen und **Engineering-Hub** auswählen. Nicht den übergeordneten Ordner öffnen, wenn die Notizen eindeutig innerhalb dieses Vaults aufgelöst werden sollen. Danach [[00-Dashboard/Home]] öffnen. Es wurde keine bestehende Obsidian-Konfiguration übernommen; damit werden keine fremden Fensterzustände oder rechnerabhängigen Einstellungen mitkopiert.

## Aufbau
- [[00-Dashboard/Home]] und [[00-Dashboard/Agenten-Übersicht]]: Einstieg und Rollen.
- [[02-Standards/README]]: allgemeine, modulare Standards mit MUSS, SOLLTE und Projektentscheidungen.
- [[03-Projects/README]]: Projektwissen; `_Example-Project` enthält ausschließlich Demonstrationsdaten.
- [[04-Workflows/single-agent-development]]: Standardbetrieb mit einem Agenten. [[04-Workflows/multi-agent-development]] nur ausdrücklich aktivieren.
- [[05-Tasks/README]]: Aufgaben, Abhängigkeiten und Reviews; Demo-Aufgaben sind keine echten Aufträge.
- [[06-Knowledge/README]]: unverbindliches Hintergrundwissen.
- [[07-Templates/README]]: manuell auszufüllende Vorlagen; optional Core-Plugin Vorlagen mit Ordner `07-Templates` verwenden.
- [[08-Codex/project-integration]]: Projektanbindung und kurze AGENTS-Vorlagen.
- [[specs/README]]: optionale Spring-Boot-Projektbasis mit ausdrücklich aktivierbaren Erweiterungen.
- [[10-Skills/README]]: Methoden-Katalog, Agent-Skills und Vorlagen.
- [[09-Maintenance/consolidation-report]]: Quellenwahl, Unterschiede und Prüfergebnis.

## Neues Projekt
Projektstruktur unter `03-Projects` nach dem Beispiel anlegen und Projektvorlage ausfüllen. Fachliche Anforderungen, akzeptierte ADRs und Prüfbefehle selbst festlegen. Zentrale Standards nur referenzieren; lokale Abweichungen dokumentieren. Für Spring-Boot-Projekte optional die Kopierzuordnung in [[specs/README]] anwenden und Erweiterungen in der Projekt-Spec aktivieren.

## Regelquellen
Allgemeine Standards werden nur unter `02-Standards` gepflegt. Konkrete Regeln der optionalen Spring-Boot-Basis werden nur unter `specs` gepflegt. Die Auswahl- und Konfliktregeln stehen in [[09-Maintenance/rule-sources]]. Beispiele, importierte Texte und ursprüngliche Prompts sind keine Nutzeraufträge.

## Codex und Kontext
Die AGENTS-Datei muss im echten Softwareprojekt liegen; der Vault wirkt nicht automatisch auf externe Projekte. Nur für die Aufgabe relevante Standards laden. Kleine Änderung: direkt im Einzelmodus arbeiten, passende Tests ergänzen und Ergebnisse berichten. Neue Rolle über [[07-Templates/agent-template]] definieren; neue Architekturregeln zentral oder als Projekt-ADR festhalten.

Beispiel: „Erweitere den Kafka Consumer um Fehlerbehandlung und Tests. Beachte Projektregeln und betroffene ADRs; arbeite als einzelner Agent.“ Weitere Beispiele: [[08-Codex/prompt-examples]].

## Grenzen
Keine Orchestrierungssoftware, Installation, Netzwerkabfrage, Git-Initialisierung oder automatische Deploymentintegration. Obsidian-Oberfläche und Bases-Syntax wurden nicht geprüft; bei Bedarf eine Base in der installierten Oberfläche erstellen, statt eine ungetestete Datei zu übernehmen. Angaben zu früheren Prüfungen sind historische Dokumentation, keine aktuelle Verifikation.
