# Codex-Integration und Nachweisgrenzen

## AGENTS.md-Konvention
Projektweite Regeln gehören in die `AGENTS.md` im echten Projektstamm; Regeln für `backend/` und `infrastructure/` in deren eigene `AGENTS.md`. Spezifische Regeln ergänzen den jeweiligen Teilbereich; zentrale Regeln nicht redundant kopieren. Bei Widersprüchen Klärung herbeiführen.

Eine AGENTS.md im Vault gilt nicht automatisch für Projekte außerhalb ihres Verzeichnisbereichs. Wikilinks, Rollen und externe Markdown-Dateien sind keine automatische Registrierung und kein Importmechanismus. Eine Leseanweisung in der Projekt-AGENTS macht die benötigten Dateien explizit zugänglich; Zugriffsrechte und tatsächlich gelesenen Kontext separat prüfen.

## Lokal geprüft am 2026-10-20
- `codex --help` ist vorhanden und nennt `-C/--cd`, `--worktree` und `--add-dir`.
- `--add-dir` erlaubt laut lokaler Hilfe zusätzliche **Schreibverzeichnisse**. Es ist kein Dokumentimport und wird für reine Standardslektüre nicht benötigt.
- Die lokal gebündelte OpenAI-Docs-Referenz `references/codex-self-knowledge.md` beschreibt Repository-AGENTS als dauerhafte Teamregeln und verschachtelte AGENTS als spezifischere Teilbereichsregeln.
- Keine Agentensitzung zum automatischen Laden gestartet; genaue Suchreihenfolge, Größenlimits, globale Overrides, Symlink-Verhalten und Versionsunterschiede wurden nicht praktisch verifiziert und werden nicht vorausgesetzt.
- Kein Netzwerkzugriff, keine Installation. Öffentliche Referenz für spätere manuelle Prüfung: [OpenAI AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md). Diese Seite wurde bei der Erstellung nicht abgerufen.

## Selektiver Kontext
Die Projekt-AGENTS enthält kurze dauerhafte Regeln und eine Lesematrix. Der Agent liest gemeinsame Grundregeln sowie nur die betroffenen Technologiestandards und Projektentscheidungen. Rollen sind optional und ändern keine Berechtigungen. Weitere Schritte: [[08-Codex/project-integration]].
