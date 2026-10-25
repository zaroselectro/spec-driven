# Konkrete Codex-Prompts

Projekt zuerst gemäß [[08-Codex/project-integration]] anbinden. Pfade im Prompt an das echte Projekt anpassen. Folgende Beispiele sind keine realen Arbeitsaufträge.

## 1. Kafka-Fehlerbehandlung — Single-Agent
„Erweitere den Kafka Consumer um geeignete Fehlerbehandlung und erforderliche Tests. Lies die geltenden AGENTS-Dateien, Kafka- und Teststandards sowie betroffene ADRs. Nutze vorhandene Retry-Vorgaben; melde offene Entscheidungen. Arbeite als einzelner Agent.“

## 2. REST-Feature
„Implementiere die im Task dokumentierte REST-Erweiterung. Lies nur die betroffenen Anforderungen, REST-, Java-/Spring- und Testregeln. Prüfe Validierung, Autorisierung und Rückwärtskompatibilität. Berichte die ausgeführten Tests.“

## 3. Architekturvorschlag
„Nutze die Rolle software-architect als Einzelagent. Vergleiche für die beschriebene Konsistenzanforderung direkte Publikation und Outbox. Erstelle einen vorgeschlagenen ADR mit Konsequenzen und offenen Fragen; ändere keine akzeptierte Entscheidung.“

## 4. Code-Review
„Prüfe den bereitgestellten Diff gegen Aufgabe und einschlägige Regeln. Dokumentiere konkrete Befunde mit Stelle, Auswirkung und Priorität im Review-Template. Nenne nicht ausgeführte Prüfungen.“

## 5. Infrastruktur
„Nutze die Rolle devops-engineer als Einzelagent. Prüfe die vorhandenen Helm-Manifeste auf Secrets, Ressourcen und Healthchecks. Bereite Korrekturen und Rollbackhinweise vor. Führe kein Deployment aus.“

## 6. Frontend
„Nutze frontend-developer als Einzelrolle. Ergänze für das dokumentierte Feature Lade- und Fehlerzustände mit passenden Tests. Beachte API-Vertrag und Angular-/TypeScript-Standard; erfinde keine UI-Anforderungen.“

## 7. Expliziter Teamauftrag
„Multi-Agent aktivieren für das beauftragte Feature. Erstelle eindeutige Aufgaben mit Owners, Reviewern und Abhängigkeiten. Lass notwendige Architekturentscheidungen prüfen, bevor Backend und Frontend umsetzen. QA und Security prüfen die Ergebnisse; DevOps bereitet Deployment vor. Nutze isolierte Branches/Worktrees, sofern die Laufzeit das unterstützt. Keine Produktionsänderung ohne Freigabe. Wenn keine Orchestrierung vorhanden ist, liefere manuelle Arbeitsaufträge und Übergaben statt eine Ausführung zu behaupten.“
