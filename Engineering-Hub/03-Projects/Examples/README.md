# Neue Projekte: zwei Arbeitsweisen

Diese Anleitungen sind Demonstrationsbeispiele, keine Aufträge zum Erzeugen einer Anwendung. Projektnamen und fachliche Regeln müssen für ein echtes Vorhaben ersetzt werden.

| Arbeitsweise | Wer steuert die nächsten Schritte? | Verwendete Inhalte |
| --- | --- | --- |
| [[03-Projects/Examples/agentic-project|Agentisch ein Projekt anlegen]] | Ein KI-Agent setzt einen abgegrenzten Auftrag um, bearbeitet Dateien und führt erlaubte Prüfungen aus. | Projekt-Spec, ausdrücklich gewählte Basis/Erweiterungen, Projektregeln und relevante Standards |
| [[03-Projects/Examples/selected-specs-project|Ohne agentischen Ablauf mit ausgewählten Specs]] | Du planst, implementierst und prüfst selbst; eine KI hilft optional mit einzelnen Antworten oder Codevorschlägen. | Nur explizit ausgewählte, unabhängige Regeln; keine automatische Übernahme der vollständigen Basis |

Beide Wege können spezifikationsgetrieben sein. „Agentisch“ bedeutet hier selbständige Umsetzung innerhalb des Auftrags, nicht zwingend mehrere Agenten. Der zweite Weg funktioniert auch vollständig ohne KI.

Quellcode liegt im tatsächlichen Softwareprojekt. Unter `03-Projects/<projekt>/` im Vault liegen dessen Dokumentation und Verweise; kein zweiter Quellcodebestand. Die AGENTS-Integration ist unter [[08-Codex/project-integration]] beschrieben.
