# Kopiervorlage für ein Agent-Skill-Paket

Neuen Ordner `.agents/skills/<skill-name>/` anlegen und den folgenden Block als `SKILL.md` speichern. Alle Platzhalter ersetzen. Name: Kleinbuchstaben, Ziffern und Bindestriche, unter 64 Zeichen; Ordnername und `name` gleich halten. Nur bei konkretem Bedarf Ressourcen ergänzen. Anleitung: [[10-Skills/agent-skills]].

```markdown
---
name: replace-with-skill-name
description: MANUELL konkrete Fähigkeit und eindeutigen Einsatzfall beschreiben.
---

# MANUELL Titel

## Eingaben und Umfang
MANUELL benötigte Eingaben, Grenzen und Umgang mit fehlenden Informationen.

## Vorgehen
MANUELL aufgabenspezifische Schritte und Entscheidungskriterien; Quellen über auflösbare relative Markdown-Links anbinden.

## Ergebnis und Prüfung
MANUELL erwartetes Ergebnis und überprüfbare Qualitätskriterien. Ausgeführte Prüfungen und verbleibende Lücken getrennt ausweisen.
```

Vor Übernahme Frontmatter, Verweise und Verhalten mit passenden sowie unpassenden Aufträgen prüfen. Die Vorlage enthält absichtlich keine ausführbaren Skripte oder zusätzlichen Metadaten.
