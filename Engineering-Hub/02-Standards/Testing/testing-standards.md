# Tests mit JUnit 5, Mockito und Cucumber

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Tests aus Akzeptanzkriterien und Risiken ableiten. Fehler-, Grenz- und Wiederholungsfälle berücksichtigen. Ergebnisse ehrlich als bestanden, fehlgeschlagen oder nicht ausgeführt angeben. Tests dürfen keine unkontrollierten externen Seiteneffekte erzeugen.

## Empfehlungen (SOLLTE)
JUnit 5 für Verhaltenstests, Mockito für echte externe Kollaboratoren, Cucumber für gemeinsam verständliche fachliche Szenarien einsetzen. PostgreSQL/Kafka-Verhalten mit geeigneten realistischen Integrationstests prüfen, etwa vorhandenen Testcontainers.

## Projektentscheidungen
Testpyramide, Integrationsumgebung, Coverage-Ziele und CI-Laufzeiten bestimmen. Werkzeuge nur bei Projektbedarf hinzufügen; in diesem Vault wird nichts installiert. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
