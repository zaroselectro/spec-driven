# Beispiel A: Neues Projekt agentisch anlegen

**Demonstration:** Eine interne Aufgabenverwaltung `task-service` soll entstehen. Die folgenden Anforderungen sind ausschließlich Beispielentscheidungen. Ein einzelner Agent genügt; andere Agenten werden nicht automatisch gestartet.

## 1. Ziel und Umfang festlegen

Für dieses Beispiel: Aufgaben anlegen und über ihre UUID abrufen, mit Titel von 1–120 Zeichen. HTTP-API, relationale Persistenz mit PostgreSQL und versionierte Migrationen. Kein Frontend, Kafka oder Deployment. Authentifizierung ist bewusst nicht enthalten: nur lokale Entwicklung, keine Veröffentlichung oder produktive Nutzung. Vor jeder externen Bereitstellung muss ein Sicherheitskonzept entschieden werden.

Beispielhafte Akzeptanzkriterien:
- `POST /tasks` mit gültigem Titel liefert `201` und eine UUID.
- `GET /tasks/{id}` liefert die angelegte Aufgabe oder `404` bei unbekannter UUID.
- Leerer beziehungsweise zu langer Titel liefert `400` mit nachvollziehbarem Fehlervertrag.
- Aufgaben bleiben nach einem Neustart erhalten; Migrationen sind nachvollziehbar.
- Unit- und erforderliche Integrationstests belegen Erfolgs- und Fehlerfälle. H2-Tests belegen nicht allein die PostgreSQL-Kompatibilität.

## 2. Projektablage vorbereiten

Ein neues, leeres Softwareverzeichnis `task-service/` wählen; bestehende Dateien zuerst prüfen. Kein Git-Repository oder externe Infrastruktur ohne entsprechenden Auftrag anlegen. Im Vault bei Bedarf `03-Projects/task-service/` nach [[03-Projects/README]] erstellen und den tatsächlichen Quellcodepfad vermerken.

Aus diesem Hub manuell oder im ausdrücklich beauftragten Vorbereitungsschritt kopieren:

| Quelle im Hub | Ziel im Softwareprojekt |
| --- | --- |
| `specs/base/AGENTS.md` | `AGENTS.md` |
| `specs/base/architecture.md` | `specs/architecture.md` |
| `specs/templates/project-spec.md` | `../../../../project.md` |
| `specs/extensions/http-api.md` | `specs/extensions/http-api.md` |
| `specs/extensions/persistence-jpa.md` | `specs/extensions/persistence-jpa.md` |

Die AGENTS-Datei um **Single-Agent als Standard**, konkrete Projektpfade und relevante Standardverweise ergänzen. Bestehende Regeln zusammenführen, niemals blind überschreiben. Die Basis bringt bewusst zusätzliche Entscheidungen wie Lombok, Package-Struktur und Maven-Testablauf mit; diese vor Auswahl lesen. Details nicht nochmals in einer zweiten Standardsammlung pflegen.

## 3. Projekt-Spec vollständig ausfüllen

In `../../../../project.md` die Vorlage ersetzen, insbesondere:

```text
Projekt: task-service (Demonstration)
Java-Basispaket: de.example.taskservice
Fachlichkeit: Aufgaben mit UUID und Titel anlegen und abrufen
Aktive Erweiterungen:
- http-api: specs/extensions/http-api.md
- persistence-jpa: specs/extensions/persistence-jpa.md
Nicht aktiviert: spring-security; keine weiteren Erweiterungen
Datenbank: PostgreSQL; Zugangsdaten ausschließlich extern
Migrationen und Tests: gemäß gewählter Basis und persistence-jpa
API: POST /tasks, GET /tasks/{id}; Fehlervertrag konkret festlegen
Betriebsumfang: ausschließlich lokal; keine öffentliche Bereitstellung
Vorlagenstand: tatsächliche Quelle, Datum, Dateiliste und überprüfbarer Stand
```

Zusätzlich alle offenen Entscheidungen der beiden Erweiterungen ausfüllen, etwa Fehlerformat und Datenbank-/Testkonfiguration. Die Kurzfassung oben ersetzt diese Detailfelder nicht. Beispiel-Akzeptanzkriterien aus Schritt 1 übernehmen. Ohne echten Commitstand einen archivierten Dateistand mit Hashes referenzieren, keinen Commit erfinden. Nicht benötigte Optionen ausdrücklich kennzeichnen.

## 4. Konkreten KI-Auftrag geben

Erst nach vollständiger Vorbereitung den Agenten im tatsächlichen Projektverzeichnis starten. Ein möglicher Auftrag:

> Lies die geltende AGENTS.md sowie specs/project.md, specs/architecture.md und die ausdrücklich aktivierten Erweiterungen. Prüfe Voraussetzungen und offene Entscheidungen. Implementiere anschließend das dort beschriebene task-service-Projekt einschließlich der erforderlichen Tests. Arbeite als einzelner Agent und bleibe im beschriebenen Umfang. Verwende nur erlaubte lokale Prüfungen; installiere oder starte keine zusätzliche Infrastruktur ohne entsprechenden Auftrag. Melde fehlende Voraussetzungen, statt sie zu umgehen. Berichte geänderte Dateien, ausgeführte Befehle, Testergebnisse und verbleibende Punkte. Erzeuge keine zusätzlichen Features und führe kein Deployment aus.

Die konkrete Spring-Boot-Version muss gemäß Basis geprüft und fixiert werden. Ist diese Prüfung in der Umgebung nicht möglich, die Einschränkung offenlegen und die Version ausdrücklich entscheiden lassen.

## 5. Ergebnis abnehmen

Diff gegen Spec prüfen, Tests und deren tatsächliche Ausführung nachvollziehen und offene Punkte bewerten. Nicht ausgeführte Tests sind kein Erfolgsnachweis. Review nach [[04-Workflows/code-review]] dokumentieren. Für ein weiteres Feature denselben kleinen Ablauf wiederholen; Multi-Agent bleibt eine ausdrückliche Option.

**Siehe auch:** [[03-Projects/Examples/selected-specs-project]] · [[specs/README]] · [[04-Workflows/single-agent-development]]
