# Architekturprinzipien

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Systemgrenzen, Verantwortlichkeiten und Abhängigkeiten müssen nachvollziehbar sein. API, Geschäftslogik und Persistenz trennen; fachliche Logik nicht in Controllern bündeln. Akzeptierte ADRs beachten; Änderungen zunächst als Vorschlag dokumentieren.

## Empfehlungen (SOLLTE)
SOLID, Separation of Concerns und Dependency Inversion gezielt nutzen. Ein modularer Monolith ist oft ein sinnvoller Ausgangspunkt; keine unnötigen Services.

## Projektentscheidungen
Komponentenschnitt, Schichtenmodell, Konsistenz, Qualitätsziele und Kommunikationsmuster mit Alternativen in ADRs entscheiden. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
