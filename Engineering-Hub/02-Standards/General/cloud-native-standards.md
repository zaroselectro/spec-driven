# Cloud-native Engineering Standards

## Geltungsbereich

Verbindliche Basis für deployte Services und deren ausgehende Netzwerkkommunikation. Ressourcenregeln gelten für Kubernetes-Workloads. Bei Änderungen nur den betroffenen Bereich prüfen; eine lokale Änderung verlangt keinen Umbau unberührter Systeme. Diese Betriebs- und Engineering-Regeln ergänzen die [[02-Standards/Cloud-Design-Patterns/README|Cloud Design Patterns]].

## Verbindliche Basisstandards (MUSS)

| Standard | Verbindliche Regel |
| --- | --- |
| Timeout | Jeder ausgehende Netzwerkaufruf muss zeitlich begrenzt sein. Verbindungsaufbau, Antwort und Gesamtoperation einschließlich Wiederholungen erhalten geeignete endliche Budgets. Bei langlebigen Streams einzelne Verbindungs-, Warte- und Wiederverbindungsoperationen begrenzen. |
| Retry Policy | Für jede ausgehende Anbindung und Nachrichtenverarbeitung muss eine Policy festgelegt sein, auch wenn sie ausdrücklich keine Wiederholung vorsieht. Erlaubte Fehlerklassen, maximale Versuche, Gesamtzeitbudget und Verhalten nach Ausschöpfung definieren. Wiederholungen müssen bezüglich ihrer Nebenwirkungen sicher sein; kein pauschaler Retry aller Fehler. |
| Health und Readiness | Jeder deployte Service muss geeignete Signale für Lebensfähigkeit und Verarbeitungsbereitschaft bereitstellen. Liveness und Readiness getrennt bewerten; ein Ausfall einer entfernten Abhängigkeit darf nicht allein eine Neustartschleife über die Liveness-Prüfung auslösen. |
| Observability | Logs, Metriken und verteilte Traces müssen eine Fehleranalyse ermöglichen. Kontext über Service- und Nachrichtengrenzen weitergeben; Fehler, Latenz, Sättigung und fachlich relevante Verarbeitung nachvollziehbar machen. Sensible Daten schützen. Sampling und Aufbewahrung im Projekt bestimmen. |
| Graceful Shutdown | Services müssen kontrolliert beendet werden können: neue Arbeit stoppen, Bereitschaft zurücknehmen und laufende Arbeit innerhalb einer definierten Frist abschließen oder sicher wiederaufnehmbar hinterlassen. Keine vorzeitige Bestätigung unverarbeiteter Nachrichten. |
| Ressourcenbegrenzung | Kubernetes-Workloads müssen definierte CPU-/Memory-Requests und angemessene Limits besitzen. Werte auf Container bzw. unterstützter Pod-Ebene passend zur Zielumgebung festlegen und durch Lastbeobachtung begründen. |
| Konfigurationsmanagement | Umgebungsabhängige Konfiguration muss vom Anwendungscode getrennt und beim Start validiert werden. Secrets nicht im Quellcode oder in versionierter Klartextkonfiguration ablegen. |

## Empfehlungen (SOLLTE)

Bei erlaubten Retries Backoff mit Obergrenze und Jitter einsetzen. Wiederholungen in Anwendung, Client und Infrastruktur gemeinsam budgetieren. Ein Timeout beweist nicht, dass eine entfernte Operation ohne Wirkung blieb.

## Projektentscheidungen

Konkrete Zeitbudgets, Retry-Fehlerklassen, Probe-Verhalten, Shutdown-Fristen, Ressourcenwerte und Observability-Konfiguration festlegen. Shutdown-Frist von Anwendung und Plattform abstimmen. Bei Kafka auch Polling, Rebalancing und Offset-Verhalten berücksichtigen. Abweichungen mit Begründung und Geltungsbereich als Projekt-ADR dokumentieren.

## Prüfung

Für betroffene Anbindungen langsame/ausbleibende Antworten, ausgeschöpfte Retry-Budgets und sichere Seiteneffekte prüfen. Für Deployment-Änderungen Bereitschaft, kontrollierte Beendigung unter laufender Last und Ressourcenwerte prüfen. Mindestens einen relevanten Fehler über Logs, Metriken und Trace-Kontext nachvollziehen. Art und Umfang der Nachweise an der Änderung ausrichten.

**Siehe auch:** [[02-Standards/Java/spring-boot-standards]] · [[02-Standards/Infrastructure/kubernetes-standards]] · [[02-Standards/General/security-principles]] · [[02-Standards/Observability/monitoring-standards]] (zusätzliche Regeln bei gewähltem Prometheus-Monitoring).
