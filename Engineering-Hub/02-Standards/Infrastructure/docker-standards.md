# Docker

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Images ohne eingebettete Secrets bauen; reproduzierbare Basisversionen festlegen. Prozess nach Möglichkeit ohne Root-Rechte betreiben; begründete Ausnahmen dokumentieren. Build-Kontext begrenzen.

## Empfehlungen (SOLLTE)
Multi-Stage-Build, kleines Laufzeitimage, explizite Healthchecks und kontrollierte Image-Prüfung verwenden. Signals und geordnetes Herunterfahren testen.

## Projektentscheidungen
Registry, Image-Updateprozess, unterstützte Plattformen und Ressourcenlimits festlegen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
