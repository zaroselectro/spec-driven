# Änderungen prüfen

> Lesefassung für Obsidian. Quelle: `.agents/skills/hub-code-review/SKILL.md`. Operative Änderungen in der Quelldatei pflegen und diese Lesefassung anschließend aktualisieren.

Aufruf im Agenten: `$hub-code-review`.

Bestimme zuerst das Review-Ziel aus dem Auftrag: uncommittete Änderungen, benannter Commit, Vergleichsbranch oder bereitgestellter Diff. Ist die Basis nicht ableitbar, kläre sie vor einem Urteil über die Änderung.

Lies den Diff und den notwendigen Kontext, geltende Projektregeln, Akzeptanzkriterien und betroffene ADRs. Nutze den [Review-Workflow](../../04-Workflows/code-review.md) und nur die fachlich einschlägigen [Standards](../../02-Standards/README.md). Eine formale Hub-Aufgabe ist nur nötig, wenn der Review in dessen Aufgabenverwaltung eingebunden ist.

Untersuche geändertes Verhalten und betroffene Aufrufer auf konkrete Fehler, Regressionen und relevante Sicherheits- oder Betriebsfolgen. Belege Befunde mit Fundstelle, Auslöser und Auswirkung. Trenne nachweisbare Probleme von offenen Fragen und optionalen Verbesserungsvorschlägen.

Führe vorhandene, für den Review relevante Prüfungen aus, soweit Werkzeuge und Umgebung das erlauben. Unterscheide ausgeführte Prüfungen, reine Codelektüre und verbleibende Prüflücken. Ohne Änderungsauftrag bleiben Code und Aufgabenstatus unverändert.

Berichte Befunde nach Schwere mit Datei und Zeile sowie einer konkreten Empfehlung; nutze bei gewünschtem Review-Dokument die [Review-Vorlage](../../07-Templates/review-template.md). Wenn keine belastbaren Befunde vorliegen, sage dies ausdrücklich und nenne verbleibende Risiken. Weise eine Prüfung eigener Änderungen als Selbstprüfung aus. Der Skill startet keine weiteren Agenten und führt keine Zusammenführung durch.
