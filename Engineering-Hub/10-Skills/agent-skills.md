# Agent-Skills erstellen und nutzen

## Aufbau eines Pakets

```text
.agents/skills/<skill-name>/
  SKILL.md                 # Pflicht: Metadaten und Anleitung
  references/              # optional: bedarfsweise gelesene Quellen
  scripts/                 # optional: ausführbare Hilfsmittel
  assets/                  # optional: Vorlagen und Ergebnisbausteine
  agents/openai.yaml       # optional: Oberfläche und Aufrufrichtlinie
```

`SKILL.md` beginnt mit YAML-Frontmatter mit `name` und `description`. Die Beschreibung benennt Fähigkeit und Auslöser präzise. Der Inhalt beschreibt die benötigten Eingaben, das Vorgehen, Ergebnis und Qualitätskriterien. Nur tatsächlich benötigte Zusatzdateien anlegen. Die [[07-Templates/agent-skill-template]] ist eine Kopiervorlage und selbst kein registriertes Paket.

## Erkennung und Aufruf in Codex

Codex sucht lokale Repository-Skills unter `.agents/skills` vom Arbeitsverzeichnis bis zur Repository-Wurzel. Zunächst werden Name und Beschreibung bereitgestellt; die vollständige Anleitung wird bei Auswahl gelesen. Auswahl ist explizit oder passend zur Beschreibung implizit möglich. In CLI und IDE können Skills mit `$skill-name` angesprochen werden; die konkrete Auswahloberfläche hängt vom Produkt ab.

Im Engineering-Hub liegen drei Pakete:

```text
.agents/skills/hub-requirements-analysis/SKILL.md
.agents/skills/hub-code-review/SKILL.md
.agents/skills/implement-from-spec/SKILL.md
```

Beispielaufträge für eine Sitzung im Hub:

> Nutze $hub-requirements-analysis, um aus dieser Idee prüfbare Anforderungen abzuleiten: …

> Nutze $hub-code-review und prüfe die uncommitteten Änderungen auf Fehler und Regressionen. Berichte Befunde mit Fundstellen.

> Nutze $implement-from-spec und implementiere die Spec `<pfad-zur-spec>` im Projekt `<zielprojekt>`. Berücksichtige aktive Regeln und ADRs; validiere mit Tests, Review und Soll-Ist-Abgleich.

Verfügbarkeit im Skill-Selektor bzw. in der Sitzung prüfen. Codex erkennt Änderungen laut Dokumentation automatisch; bei fehlender Anzeige neu starten. Eine erfolgreich gelesene Datei allein belegt noch keine automatische Erkennung. Wikilinks im Vault installieren oder registrieren keinen Skill.

## In ein anderes Projekt übernehmen

Die Pakete dieses Hubs verwenden relative Markdown-Links auf Hub-Standards, Workflows und Vorlagen. Sie sind für die vorhandene Verzeichnisstruktur geschrieben. Sie wirken nicht automatisch auf externe Projekte.

1. Gewünschten Skill-Ordner nach `<projekt>/.agents/skills/` kopieren; vorhandene Pakete vergleichen und Änderungen gezielt zusammenführen.
2. Alle referenzierten Hub-Dateien prüfen. Entweder die nötigen Quellen einschließlich ihrer Abhängigkeiten mitnehmen oder die Links im kopierten `SKILL.md` auf die geltenden Projektdateien umstellen. Die relativen Hub-Pfade funktionieren bei einer isolierten Kopie nicht.
3. Projektspezifische Anforderungen und akzeptierte ADRs anbinden; bestehende Projektregeln berücksichtigen. Name und Beschreibung bei geänderter Fähigkeit anpassen.
4. Herkunft, Übernahmedatum und konkreten Quellstand im Projekt dokumentieren. Bei lokaler Anpassung gibt es keine automatische Rücksynchronisierung.
5. Frontmatter und Verweise prüfen; danach in einer neuen oder aktualisierten Sitzung Erkennung und Verhalten mit einem begrenzten Beispielauftrag verifizieren.

Für persönliche Nutzung nennt die aktuelle Dokumentation auch `~/.agents/skills`. Repository-Skills halten Teamwissen beim Projekt. Produkt- und versionsabhängige Suchorte vor einer Installation prüfen; diese Ergänzung installiert keine globalen Skills.

## Qualität und Grenzen

- Positive Probe: Der Skill liefert das vereinbarte Ergebnis bei passendem Auftrag.
- Negative Probe: Ein ähnlicher Auftrag außerhalb der Beschreibung soll nicht unnötig den Skill auswählen.
- Fehlende Eingaben: Nachfragen nur bei entscheidenden Lücken; unabhängige Teile bearbeiten und Annahmen sichtbar machen.
- Quellen und Skripte prüfen, bevor sie genutzt werden. Ausführung unterliegt den vorhandenen Werkzeugen und Berechtigungen; eine Anleitung erteilt keinen zusätzlichen Auftrag für externe Aktionen.
- Single-Agent bleibt Standard. Ein Skill aktiviert weder Delegation noch Deployment.

## Quelle und Prüfstand

Technische Angaben am 2026-10-28 anhand von [OpenAI: Build skills](https://learn.chatgpt.com/docs/build-skills) geprüft. Struktur und interne Verweise der lokalen Pakete wurden separat geprüft; automatische Erkennung und Verhalten wurden nicht in einer neuen Codex-Sitzung getestet.
