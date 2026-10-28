# Ein bestehendes Projekt anbinden

Diese Anleitung wird manuell angewendet; während der Vault-Erstellung wurde kein bestehendes Projekt verändert.

1. Bestehende Projekt- und Unterverzeichnis-AGENTS sowie Teamvorgaben lesen. Vorhandene Dateien nur nach ausdrücklicher Genehmigung zusammenführen, nicht überschreiben.
2. Eine freigegebene Revision der benötigten zentralen Standards in ein projektlokales Verzeichnis `engineering-standards/` kopieren, mit ursprünglichem Vault-Pfad, Prüfumfang und Kopierdatum in einer Herkunftsnotiz. Dies ist ein bewusst versionierter Snapshot, keine zweite Quelle für Regelpflege. Änderungen zentral pflegen und kontrolliert synchronisieren.
3. Projektweite Vorlage `templates/project.AGENTS.md.template` als `AGENTS.md` im Projektstamm anlegen. Alle MANUELL-Felder ersetzen und Pfade relativ zum Projektstamm prüfen.
4. Bei entsprechenden Verzeichnissen Backend- und Infrastrukturvorlage als `backend/AGENTS.md` und `infrastructure/AGENTS.md` anlegen. Dateinamen der Standards im Snapshot beibehalten.
5. Projektspezifische Buildbefehle, Architekturpfade und Randbedingungen eintragen. Nur echte Projektangaben übernehmen. Rollen bei Bedarf ebenfalls lokal kopieren oder über einen geprüften lesbaren Pfad explizit referenzieren.
6. Codex im Projektstamm starten. Erste Aufgabe: „Nenne die geltenden AGENTS-Dateien und lies die Grundregeln. Liste die für diese Aufgabe erforderlichen Standarddateien mit Pfad; melde fehlende Dateien.“ Ergebnis gegen Dateien prüfen. Fehlende Regeln nicht still übergehen.

## Alternative: zentralen Vault direkt lesen
Statt Snapshot kann die Projekt-AGENTS einen konkreten absoluten lokalen Vault-Pfad mit expliziten Leseanweisungen enthalten. Vorher Lesbarkeit in der verwendeten Laufzeit prüfen. Dies ist weniger portabel und koppelt Projekte an laufende zentrale Änderungen. Keine automatische externe Erkennung behaupten; keine Symlinks oder zusätzlichen Schreibrechte dafür einrichten. Bei fehlendem Zugriff auf den überprüfbaren Snapshot zurückgreifen.

## Pflege und Rollen
Snapshot-Stand bei Aufgabenbeginn identifizieren und nach zentralen Änderungen gezielt aktualisieren. Keine stillen Aktualisierungen während eines laufenden Reviews. Rolle im Prompt selektiv aktivieren: „Lies zusätzlich die lokal bereitgestellte Rolle backend-developer; bleibe ein einzelner Agent.“ Dauerhafte Regeln bleiben in AGENTS und Standards, nicht ausschließlich in einer optionalen Rolle.

## Vorlagen
Die Dateien liegen im Unterordner `08-Codex/templates` und enden absichtlich auf `.template`; sie müssen vor Nutzung manuell kopiert und ausgefüllt werden. Sie sind keine aktiven AGENTS-Dateien im Zielprojekt.


## Optionale Spring-Boot-Specs
Die Sammlung liegt in diesem Vault unter `specs/`, siehe [[specs/README]]. Für ein neues Projekt die dortige Kopierzuordnung verwenden: `specs/base/AGENTS.md` wird Projekt-AGENTS, `specs/base/architecture.md` wird `specs/architecture.md`, und die Projektvorlage wird `../../../project.md`. Nur gewählte Erweiterungen übernehmen und ausdrücklich aktivieren. Bei vorhandener AGENTS Inhalte nach Konfliktprüfung zusammenführen, nie blind ersetzen. Die allgemeine AGENTS-Vorlage ergänzt Arbeitsmodus, Standardpfade und Projektangaben; sie ersetzt nicht die gewählte Specs-Basis.

Die Basisdatei der Sammlung ist ausführlicher als die allgemeine Einstiegsvorlage. Für dauerhafte Nutzung die Regeln bei Bedarf in explizit zu lesende Projekt-Specs auslagern; keine Details ohne Prüfung streichen. Snapshot-/Quellstand einschließlich Dateihashes dokumentieren. Zuständigkeiten: [[09-Maintenance/rule-sources]].
