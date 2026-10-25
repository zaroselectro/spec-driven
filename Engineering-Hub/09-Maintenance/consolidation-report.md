# Zusammenführungsbericht

Stand: 2026-10-25. Neuer unabhängiger Vault `Engineering-Hub`; Originalordner bleiben unverändert.

## Auswahl
- `mimo`: Navigation, sieben ausführliche Rollen, sechs Workflows, Projektbeispiel, Aufgaben und Wissensnotizen.
- `software-engineering`: allgemeine Standards, manuell nutzbare Templates und konservativ dokumentierte Codex-Integration mit drei `.md.template`-Dateien.
- `foo` und `mimo`: identische Specs-Sammlung, einmal aus `mimo` übernommen. `foo`-Varianten sind durch ausführlichere Rollen und Workflows abgedeckt. Abweichende technische Defaults bleiben ausschließlich in der ausdrücklich wählbaren Specs-Basis.
- Historische Prompts, alte Prüfberichte, Git-Verzeichnisse, Obsidian-Fensterzustände und der leere `codex`-Ordner wurden nicht als aktive Inhalte übernommen. Originale sind weiter in den Quellordnern vorhanden.

## Bewusste Bereinigungen
Ein Vault als eindeutige Linkwurzel; Specs liegen unter `specs/` innerhalb desselben Vaults. Links und Vorlagenverweise angepasst. Keine doppelte aktive Standardsammlung und keine Ausführung der Rollen. Allgemeine Standards und konkrete Spring-Boot-Vorgaben sind getrennt; siehe [[09-Maintenance/rule-sources]].

## Prüfungen
- 491 Wikilinks vor dem Bericht auf vorhandene Zielnotizen geprüft.
- 7 relative Markdown-Verweise auf vorhandene Zieldateien geprüft.
- 35 YAML-Frontmatter-Blöcke mit lokal vorhandenem Ruby/Psych sicher geparst.
- Keine Software installiert, keine Netzwerkabfrage, keine externe Agentensitzung oder Orchestrierung gestartet.
- Quellen vor und nach Erstellung mittels SHA-256 vergleichen; Manifest: `09-Maintenance/source-manifest.json`.
- Obsidian-UI und automatische Codex-Kontextladung nicht funktional geprüft. Historische Codex-Befunde werden ausdrücklich nicht zu aktuellen Prüfungen erklärt.

## Weiterarbeiten
Diesen Ordner als Vault öffnen, [[00-Dashboard/Home]] verwenden und neue Änderungen hier pflegen. Bestehende Projekte werden nicht automatisch umgestellt. [[08-Codex/project-integration]] beschreibt die manuelle Anbindung. Die bisherigen Ordner können später nach Prüfung archiviert werden; hier wurde nichts entfernt.
