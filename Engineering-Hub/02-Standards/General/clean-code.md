# Clean Code

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Namen müssen Fachlichkeit ausdrücken. Fehler dürfen nicht still verschluckt werden. Ressourcen schließen und sensible Daten aus Logs fernhalten.

## Empfehlungen (SOLLTE)
Kleine kohärente Einheiten, Constructor Injection, explizite Abhängigkeiten und strukturierte Fehler bevorzugen. Kommentare erklären Gründe. Vorhandenen Stil bewahren.

## Projektentscheidungen
Formatierung, statische Analyse und zulässige Komplexität im Projekt festlegen; keine pauschalen Methodenlängen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
