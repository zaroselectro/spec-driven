---
type: standard
tags: [standards, architektur, cloud, resilience]
---

# Cloud Design Patterns

## Geltungsbereich und Auswahl

Dieser Bereich unterstützt Architektur, Implementierung und Review von Java-/Spring-Boot-Anwendungen mit Kafka und Kubernetes. Er gilt bei Änderungen an verteilten Aufrufen, Nachrichtenverarbeitung, Datenkonsistenz, Lastverhalten oder Migrationen. Nur die betroffenen Patterns betrachten; eine lokale Änderung verlangt keine Bewertung des gesamten Katalogs.

**Kein Pattern ohne konkretes Problem. Es müssen nicht immer alle Patterns eingesetzt werden.** Es gelten drei Stufen: verbindliche Basisstandards, bedingt verpflichtende Patterns und optionale Architekturpatterns. Die Priorisierung der ursprünglichen zwölf Patterns bleibt eine technische Empfehlung dieses Hubs; sie ersetzt nicht die nachfolgend definierte Verbindlichkeit und ist keine offizielle Rangliste von Azure oder AWS.

## Verbindliche Regeln (MUSS)

- Die [[02-Standards/General/cloud-native-standards|Cloud-native Engineering Standards]] einhalten. Sie sind die verbindliche Basis für Timeouts, Retry Policy, Health/Readiness, Observability, Graceful Shutdown, Ressourcen und Konfiguration.
- Für den betroffenen Bereich relevante Ausfall-, Last- und Konsistenzrisiken prüfen. Trifft eine Bedingung der folgenden Tabelle zu, ist das zugeordnete Pattern verpflichtend. Ein bloßer Hinweis auf die optionale Pattern-Auswahl genügt dann nicht als Verzichtsbegründung. Ohne erfüllte Bedingung und bei optionalen Patterns Auswahl oder bewussten Verzicht bei relevanten Risiken begründen; keine vollständige Katalogprüfung für jede Aufgabe verlangen.
- Bestehende Regeln insbesondere aus [[02-Standards/API/rest-standards]] und [[02-Standards/Messaging/kafka-standards]] bleiben gültig. Die optionale Auswahl eines Patterns hebt beispielsweise die Pflicht zur sicheren erneuten Kafka-Verarbeitung nicht auf.
- Tragende Architekturentscheidungen und Abweichungen gemäß [[02-Standards/General/architecture-principles]] im Projekt-ADR festhalten. Für kleine Änderungen innerhalb akzeptierter Entscheidungen genügt ein Hinweis in Aufgabe oder Review.

## 1. Verbindliche Basisstandards

Die fachliche Regelquelle ist [[02-Standards/General/cloud-native-standards]]. Timeout ist immer verbindlich; eine Retry Policy ist ebenfalls verbindlich, kann aber ausdrücklich keine Wiederholung vorsehen. Retry mit Backoff beschreibt die Ausgestaltung tatsächlich erlaubter Wiederholungen. Die Basisstandards sind nicht ausschließlich klassische Design Patterns.

## 2. Bedingt verpflichtende Patterns (MUSS bei erfüllter Bedingung)

| Pattern | Bedingung für verpflichtenden Einsatz |
| --- | --- |
| Circuit Breaker | Eine synchrone Abhängigkeit kann bei Ausfall die Stabilität des aufrufenden Dienstes gefährden. |
| Bulkhead | Kritische und weniger kritische Aufrufe konkurrieren um begrenzte Ressourcen. |
| Idempotent Consumer | Nachrichten werden mit At-least-once-Semantik verarbeitet und verursachen fachliche Nebenwirkungen. |
| Transactional Outbox | Datenbankänderung und Event-Veröffentlichung müssen zuverlässig gekoppelt werden und es existiert kein nachgewiesen gleichwertiger Mechanismus. |
| Rate Limiting | APIs sind durch externe oder unkontrollierte Last gefährdet. |
| Dead Letter Topic / Queue | Nicht verarbeitbare Nachrichten müssen isoliert und später untersucht oder erneut verarbeitet werden. |
| Competing Consumers | Dieselbe fachliche Kafka-Verarbeitung muss für einen gemeinsamen Nachrichtenstrom über mehrere Instanzen skaliert werden. |

Eine Umsetzung durch vorhandene Plattform- oder Client-Funktionen erfüllt die Pflicht, sofern deren Wirkung für das Projekt nachgewiesen ist; zusätzliche Bibliotheken sind nicht vorgeschrieben. Abweichungen benötigen eine ausdrücklich akzeptierte Ausnahme gemäß den Architekturprinzipien. Bei Outbox ist ein nachgewiesen gleichwertiger Kopplungsmechanismus bereits Teil der Auswahlregel.

Idempotenz richtet sich nach den Auswirkungen: Rechnungen und Zahlungen benötigen Schutz vor doppelten Geschäftswirkungen. Ein Metrik-Consumer benötigt nicht automatisch dieselbe Deduplizierungsstrategie; mögliche Doppelzählungen und zulässige Genauigkeit müssen dennoch bewertet sein. Unabhängige fachliche Abonnenten gehören in eigene Consumer Groups; sie sind keine konkurrierenden Instanzen derselben Verarbeitung.

## 3. Optionale Architekturpatterns

| Pattern | Entscheidungskriterium |
| --- | --- |
| CQRS | Benötigen Lesen und Schreiben tatsächlich unterschiedliche Modelle? |
| Event Sourcing | Muss der fachliche Zustand aus einer vollständigen Ereignishistorie rekonstruierbar sein? |
| Saga | Gibt es fachliche Transaktionen über mehrere unabhängige Services? |
| Cache-Aside | Rechtfertigt die Zugriffslast die zusätzliche Cache-Komplexität? |
| Strangler Fig | Wird ein bestehendes System schrittweise migriert? |
| Backend for Frontend (BFF) | Haben unterschiedliche Frontends stark abweichende API-Anforderungen? |
| Sharding | Reicht die Skalierung der bisherigen Datenhaltung nicht mehr aus? |

Auch bei passendem Entscheidungskriterium besteht keine pauschale Einführungspflicht. Nutzen, Alternativen und zusätzliche Komplexität im Projekt bewerten und tragende Entscheidungen als ADR festhalten.

## Priorisierung der ursprünglichen zwölf Patterns

| Pattern | Zweck | Empfehlung |
| --- | --- | --- |
| Retry mit Backoff | Temporäre Fehler abfangen | Sehr hoch |
| Circuit Breaker | Fehlerhafte Dienste abschirmen | Sehr hoch |
| Timeout | Wartezeiten begrenzen | Sehr hoch |
| Bulkhead | Fehlerausbreitung verhindern | Hoch |
| Idempotent Consumer | Doppelte Events sicher verarbeiten | Sehr hoch bei Kafka |
| Transactional Outbox | DB-Änderung und Event zuverlässig koppeln | Sehr hoch bei Kafka |
| Competing Consumers | Nachrichten parallel verarbeiten | Hoch |
| Rate Limiting | Überlastung vermeiden | Hoch |
| CQRS | Schreib- und Lesemodelle trennen | Situativ |
| Saga | Verteilte Geschäftsprozesse koordinieren | Situativ |
| Cache-Aside | Datenbankzugriffe reduzieren | Situativ |
| Strangler Fig | Altsysteme schrittweise ersetzen | Hoch bei Migrationen |

## Einsatz, Ausgestaltung und Prüfung

Die Verbindlichkeit folgt den drei Stufen oben. Die folgenden Hinweise konkretisieren die Umsetzung und nennen passende Prüfszenarien.

### 1. Retry mit Backoff

- **Einsetzen bei:** transienten Netzwerk-, Verfügbarkeits- oder Drosselungsfehlern, sofern eine Wiederholung sicher ist.
- **Ausgestaltung:** begrenzte Versuche, exponentieller Backoff mit Obergrenze und Jitter; Gesamtzeitbudget einschließlich Wartezeiten einhalten. Wiederholungen über Client, Anwendung und Infrastruktur abstimmen, damit sie sich nicht vervielfachen.
- **Grenze:** fachliche Ablehnungen und permanente Fehler nicht blind wiederholen. Bei Seiteneffekten Idempotenz absichern; ein Timeout beweist nicht, dass die Operation wirkungslos blieb.
- **Prüfen:** temporärer Fehler, ausgeschöpftes Budget und mehrfacher Aufruf mit gleichem fachlichem Ergebnis.

Quellen: [AWS Retry with backoff](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/retry-backoff.html), [AWS Timeouts, retries, and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/).

### 2. Circuit Breaker

- **Einsetzen bei:** wiederholt ausfallenden oder sehr langsamen entfernten Abhängigkeiten.
- **Ausgestaltung:** Fehler- und Langsamkeitsschwellen, Öffnungsdauer und begrenzte Testaufrufe im Half-open-Zustand festlegen; Zustandswechsel beobachten.
- **Grenze:** ersetzt keinen Timeout. Ein Fallback braucht fachlich zulässige Ergebnisse; Fehler nicht als Erfolg verschleiern. Fachliche Ablehnungen nicht pauschal als Dienstausfall zählen.
- **Prüfen:** Öffnen, schnelle Ablehnung und kontrollierte Wiederaufnahme nach Erholung.

Quelle: [Azure Circuit Breaker](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker).

### 3. Timeout

- **Einsetzen bei:** Netzwerkaufrufen und anderen potenziell langen Warteoperationen.
- **Ausgestaltung:** Verbindungsaufbau, Antwort und Gesamtaufruf passend begrenzen; Teilaufrufe und Retries in das Zeitbudget des übergeordneten Vorgangs einpassen.
- **Grenze:** ein abgebrochenes Warten beendet die entfernte Verarbeitung nicht zwangsläufig. Client-, Gateway- und Infrastrukturgrenzen aufeinander abstimmen.
- **Prüfen:** langsame oder ausbleibende Antwort, Ressourcenfreigabe und Verhalten bei unklarem Verarbeitungsergebnis.

Quelle: [AWS Timeouts, retries, and backoff with jitter](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter/).

### 4. Bulkhead

- **Einsetzen bei:** gemeinsam genutzten Ressourcen, deren Erschöpfung unabhängige Funktionen beeinträchtigen kann.
- **Ausgestaltung:** Parallelität, Warteschlangen sowie Thread- und Connection-Pools gezielt begrenzen oder trennen; Verhalten bei Sättigung definieren.
- **Grenze:** zusätzliche Pools kosten Ressourcen. Kubernetes-Replikate allein isolieren keine gemeinsam genutzte Datenbank oder erschöpfte Verbindungspools.
- **Prüfen:** eine Abhängigkeit auslasten und nachweisen, dass unabhängige Funktionen innerhalb ihrer Ziele weiterarbeiten.

Quelle: [Azure Bulkhead](https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead).

### 5. Idempotent Consumer

- **Einsetzen bei:** wiederholbarer Zustellung oder Replay, insbesondere bei Kafka mit fachlichen Seiteneffekten.
- **Ausgestaltung:** stabilen Event-/Geschäftsschlüssel und dauerhafte Deduplizierung oder gleichwertige Strategie wählen. Deduplizierungsnachweis und lokale Geschäftsänderung atomar absichern; externe Seiteneffekte gesondert schützen.
- **Grenze:** ein flüchtiges In-Memory-Set reicht über Neustarts und Replikate hinweg nicht aus. Aufbewahrungsdauer gegen Replay-Zeitraum abgleichen.
- **Prüfen:** doppelte und gleichzeitige Zustellung, Neustart sowie Absturz nach Geschäftsänderung vor Offset-Bestätigung.

Quelle: [Azure Idempotent Consumer](https://learn.microsoft.com/en-us/azure/architecture/patterns/idempotent-consumer). Verbindliche Grundlage: [[02-Standards/Messaging/kafka-standards]].

### 6. Transactional Outbox

- **Einsetzen bei:** zuverlässig zu koppelnder Geschäftsänderung und Event-Veröffentlichung, wenn kein gleichwertiger Mechanismus besteht. Kafka allein begründet noch keine Outbox.
- **Ausgestaltung:** Geschäftsänderung und Outbox-Eintrag in derselben lokalen DB-Transaktion speichern; separaten Publisher oder CDC für die Zustellung einsetzen. Wiederanlauf, Reihenfolge pro fachlichem Schlüssel und Bereinigung festlegen.
- **Grenze:** die Veröffentlichung erfolgt nachgelagert und kann Duplikate erzeugen; Consumer bleiben idempotent. Eine Outbox ist keine atomare Gesamttransaktion zwischen Datenbank und Kafka.
- **Prüfen:** DB-Rollback, Broker-Ausfall sowie Absturz nach Veröffentlichung vor deren Bestätigung in der Outbox.

Quelle: [AWS Transactional outbox](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html).

### 7. Competing Consumers

- **Einsetzen bei:** Skalierung derselben fachlichen Kafka-Verarbeitung über mehrere Instanzen für einen gemeinsamen Nachrichtenstrom; auch bei anderer unabhängig parallelisierbarer Nachrichtenverarbeitung bewerten.
- **Ausgestaltung:** bei Kafka mit klassischen Consumer Groups dieselbe Group-ID für gemeinsame Arbeit nutzen; Partitionen begrenzen die gleichzeitig aktiven Consumer. Schlüssel und Verarbeitung so wählen, dass benötigte Reihenfolge erhalten bleibt.
- **Grenze:** unterschiedliche Consumer Groups erhalten jeweils einen eigenen Nachrichtenstrom. Mehr Pods erhöhen bei ausgeschöpften Partitionen nicht die aktive Consumer-Anzahl. Parallelisierung hebt Idempotenz und Offset-Sorgfalt nicht auf.
- **Prüfen:** Lastverteilung, Rebalancing, Wiederzustellung und Reihenfolge der zusammengehörigen Events.

Quellen: [Azure Competing Consumers](https://learn.microsoft.com/en-us/azure/architecture/patterns/competing-consumers), [Apache Kafka Consumer API](https://kafka.apache.org/41/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html).

### 8. Rate Limiting

- **Einsetzen bei:** begrenzten Kapazitäten oder Quoten eingehender bzw. ausgehender Aufrufe und zur fairen Nutzung durch Clients oder Mandanten.
- **Ausgestaltung:** Rate, Burst, Bezugsgröße und Reaktion bei Überschreitung festlegen. Bei HTTP eine passende Drosselungsantwort, etwa 429 mit Retry-Hinweis, vorsehen.
- **Grenze:** ein Limit pro Pod ist kein globales Limit; Replikaskalierung berücksichtigen. Rate Limiting begrenzt die Aufrufrate, Bulkheads begrenzen gleichzeitig belegte Ressourcen.
- **Prüfen:** Lastspitzen, mehrere Replikate, Mandantenfairness und Verhalten bei Ausfall der Limit-Verwaltung.

Quelle: [Azure Rate Limiting](https://learn.microsoft.com/en-us/azure/architecture/patterns/rate-limiting-pattern).

### 9. CQRS

- **Einsetzen bei:** deutlich unterschiedlichen fachlichen Modellen oder Lastprofilen für Schreiben und Lesen.
- **Ausgestaltung:** Command- und Query-Modelle gezielt trennen. Bei asynchronen Leseprojektionen Aktualisierungsverzug, Wiederaufbau und Verhalten nach Schreibzugriffen definieren.
- **Grenze:** getrennte Datenbanken und Event Sourcing sind keine Voraussetzung. Für einfaches CRUD rechtfertigt der Nutzen den zusätzlichen Aufwand häufig nicht.
- **Prüfen:** Validierung auf der Schreibseite, Lesekonsistenz und bei Projektionen verzögerte oder wiederholte Events.

Quelle: [Azure CQRS](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs).

### 10. Saga

- **Einsetzen bei:** fachlichen Abläufen über mehrere Dienste mit jeweils lokalen Transaktionen, wenn eine einzelne lokale Transaktion nicht ausreicht.
- **Ausgestaltung:** Orchestrierung oder Choreografie, Zustände, Timeouts, idempotente Schritte, Kompensationen und manuelle Fehlerbehebung festlegen.
- **Grenze:** Kompensation ist eine fachliche Gegenaktion und kein technisches ACID-Rollback. Irreversible Schritte und zwischenzeitlich sichtbare Zustände ausdrücklich behandeln.
- **Prüfen:** Fehler in jedem Schritt, doppelte Nachrichten sowie fehlgeschlagene oder wiederholte Kompensationen.

Quelle: [Azure Saga](https://learn.microsoft.com/en-us/azure/architecture/patterns/saga).

### 11. Cache-Aside

- **Einsetzen bei:** häufig gelesenen Daten, wenn gemessene Latenz oder Datenbanklast einen Cache rechtfertigen und begrenzte Veraltung tolerierbar ist.
- **Ausgestaltung:** bei Cache-Miss aus der Datenquelle laden; TTL, Invalidierung nach Änderungen, Größenlimit und Verhalten bei Cache-Ausfall festlegen.
- **Grenze:** Cache und Datenbank können auseinanderlaufen. Gleichzeitige Misses können die Datenbank überlasten; sensible Daten und Mandantentrennung auch im Cache berücksichtigen.
- **Prüfen:** Miss, abgelaufener und veralteter Eintrag, konkurrierende Änderung sowie Cache-Ausfall unter Last.

Quelle: [Azure Cache-Aside](https://learn.microsoft.com/en-us/azure/architecture/patterns/cache-aside).

### 12. Strangler Fig

- **Einsetzen bei:** schrittweiser Ablösung eines Altsystems mit abgrenzbaren Funktionen.
- **Ausgestaltung:** Funktionen inkrementell übernehmen und Aufrufe gezielt zum alten oder neuen System routen. Datenhoheit, Vertragskompatibilität, Rückfallweg und Abschaltkriterien planen.
- **Grenze:** paralleler Betrieb erhöht Aufwand; unkoordinierte Schreibzugriffe beider Systeme auf dieselben Daten gefährden Konsistenz. Ohne Migrationsbedarf kein Einsatzanlass.
- **Prüfen:** Routing, Schnittstellenparität, Datenübergang und Rückfall innerhalb der vorgesehenen Migrationsphase.

Quelle: [Azure Strangler Fig](https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig).

### 13. Dead Letter Topic / Queue

- **Einsetzen bei:** Nachrichten, die nach der definierten Retry Policy nicht verarbeitet werden können und isoliert untersucht oder erneut verarbeitet werden müssen.
- **Ausgestaltung:** Originalnachricht und sichere Diagnoseinformationen erhalten; Zuständigkeit, Alarmierung, Zugriffsschutz, Aufbewahrung und kontrollierten Replay festlegen. In Kafka Fehlerkanal-Publikation und Offset-Bestätigung gegen Verlust absichern.
- **Grenze:** das Auslagern kann fachliche Reihenfolge unterbrechen. Bei strikter Reihenfolge das Verhalten für nachfolgende Nachrichten festlegen. Ein Fehlerkanal ohne Verantwortlichkeit und Wiederanlaufprozess genügt nicht.
- **Prüfen:** permanente Fehler, voller/unerreichbarer Fehlerkanal und Replay ohne doppelte Geschäftswirkung.

Quelle: [AWS Dead-letter queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html). Die Kafka-Prüfpunkte sind die Konkretisierung dieses Hubs.

### 14. Event Sourcing

- **Einsetzen bei:** fachlich begründeter Rekonstruktion des Zustands aus einer vollständigen Ereignishistorie.
- **Ausgestaltung:** Events als maßgebliche Datenquelle, Schemaentwicklung, konkurrierende Änderungen und deterministischen Wiederaufbau festlegen.
- **Grenze:** Kafka-Nutzung oder Audit-Logs allein sind kein Event Sourcing. Historie, Korrekturen und Datenschutz erhöhen Aufwand; ein einfacheres Zustandsmodell als Alternative prüfen.
- **Prüfen:** Zustandsrekonstruktion, alte Event-Versionen und Replay ohne erneute externe Seiteneffekte.

Quelle: [Azure Event Sourcing](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing).

### 15. Backend for Frontend (BFF)

- **Einsetzen bei:** stark unterschiedlichen API-Anforderungen verschiedener Frontends.
- **Ausgestaltung:** frontendbezogene Aggregation und Verträge abgrenzen; fachliche Regeln in den zuständigen Diensten halten.
- **Grenze:** zusätzliche Deployments, Netzaufrufe und Wartung rechtfertigen; bei ähnlichen Anforderungen reicht häufig eine gemeinsame API.
- **Prüfen:** Frontend-Verträge, Autorisierung und Ausfall einzelner aggregierter Abhängigkeiten.

Quelle: [Azure Backends for Frontends](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends).

### 16. Sharding

- **Einsetzen bei:** nachgewiesenen Skalierungsgrenzen der bisherigen Datenhaltung.
- **Ausgestaltung:** Shard-Schlüssel, Datenverteilung, Routing, Rebalancing und Wiederherstellung festlegen.
- **Grenze:** Hotspots, shardübergreifende Abfragen und Transaktionen erhöhen Komplexität. Einfachere Optimierungen vorab bewerten.
- **Prüfen:** ungleich verteilte Last, Shard-Ausfall und Datenverschiebung ohne Verlust oder falsche Zuordnung.

Quelle: [Azure Sharding](https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding).

## Projektentscheidungen

Für ausgewählte Patterns konkrete Parameter, Verantwortlichkeiten und Betriebsverhalten im Projekt festlegen: Zeit- und Retry-Budgets, Schwellenwerte, Parallelität, Deduplizierungs- und Replay-Zeiträume, Konsistenzziele sowie Wiederanlauf. Keine pauschalen Zahlen oder zusätzlichen Bibliotheken allein aus diesem Katalog ableiten.

Zusammenspiel bewerten: Timeout begrenzt Wartezeit, Retry behandelt vorübergehende Fehler, Circuit Breaker schützt bei anhaltenden Fehlern und Bulkhead begrenzt Ressourcenverbrauch. Outbox und Idempotent Consumer ergänzen sich bei zuverlässiger Ereignisverarbeitung. Reihenfolge und Zuständigkeit der Mechanismen in Anwendung, Client und Infrastruktur bewusst festlegen.

## Prüfung

Im Review die betroffenen Basisregeln, die Auslöser bedingter Pflichten, deren Erfüllung bzw. akzeptierte Ausnahmen und die begründete Auswahl optionaler Patterns prüfen. Passende Fehler- bzw. Lastnachweise heranziehen. Nur relevante Szenarien aus den obigen Abschnitten übernehmen. Metriken für das gewählte Verhalten vorsehen, etwa Timeouts, Retry-Anzahl, Breaker-Zustand, abgewiesene Aufrufe, Consumer Lag oder Outbox-Rückstand; siehe [[02-Standards/General/cloud-native-standards]] und bei gewähltem Prometheus-Monitoring [[02-Standards/Observability/monitoring-standards]].

## Quellen und Einordnung

Kataloggrundlagen: [Microsoft Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/patterns/) und [AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/). Quellen am 2026-11-11 geprüft. Auswahl, Priorisierung und Prüfpunkte sind die Einordnung für diesen Hub; konkrete Architektur und Parameter werden im Projekt entschieden.

**Siehe auch:** [[02-Standards/README]] · [[04-Workflows/architecture-design]] · [[06-Knowledge/Software-Architecture/README]]
