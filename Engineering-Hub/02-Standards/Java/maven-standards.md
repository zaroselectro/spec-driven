# Maven

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Java-Release und Dependency-/Plugin-Versionen zentral festlegen; keine dynamischen Produktionsversionen. Build darf keine Secrets enthalten. Den projektspezifischen Prüfpfad dokumentieren.

## Empfehlungen (SOLLTE)
Vorhandenen Maven Wrapper nutzen. Dependency Management beziehungsweise Spring-Boot-BOM verwenden; `verify` als Integrationspunkt vorsehen.

## Projektentscheidungen
Module, Repositories, Enforcer-Regeln und CI-Profile festlegen. Beispielbefehle `./mvnw test` und `./mvnw verify` gelten nur bei vorhandenem Wrapper; hier wird nichts ausgeführt. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
