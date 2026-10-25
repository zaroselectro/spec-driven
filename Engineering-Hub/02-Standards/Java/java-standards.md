# Java 21

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Java 21 als Zielversion im Build deklarieren. Typisierte Schnittstellen, definierte Nullbehandlung und nachvollziehbare Exceptions verwenden. Gemeinsamen veränderlichen Zustand absichern.

## Empfehlungen (SOLLTE)
Records für unveränderliche Daten, Enums für geschlossene Werte und try-with-resources verwenden, sofern passend. Virtual Threads nur nach Bewertung von Workload und Bibliotheken.

## Projektentscheidungen
Package-Struktur, Nullability-Konvention und Einsatz neuer Sprachfeatures dokumentieren. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
