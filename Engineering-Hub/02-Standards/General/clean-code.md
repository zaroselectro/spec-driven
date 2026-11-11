# Clean Code

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
- Namen müssen fachliche Bedeutung und Absicht ausdrücken; für denselben fachlichen Begriff konsistente Namen verwenden.
- Fehler dürfen nicht still verschluckt werden. Fehlerzustände eindeutig melden und an einer geeigneten Stelle behandeln; relevante Ursachen erhalten.
- Ressourcen zuverlässig schließen und sensible Daten aus Logs fernhalten.
- Von außen beobachtbare Seiteneffekte, etwa Schreibzugriffe, Nachrichtenversand oder Änderungen an übergebenen Objekten, müssen aus dem Vertrag einer Schnittstelle erkennbar sein.

## Empfehlungen (SOLLTE)
- **Klare Verantwortung und Kohäsion:** Funktionen, Klassen und Module auf einen zusammenhängenden Zweck ausrichten. Zusammengehörige Logik zusammenhalten und unabhängige Verantwortlichkeiten trennen.
- **Geringe Kopplung:** Abhängigkeiten explizit machen und auf benötigte Schnittstellen begrenzen. Constructor Injection bevorzugen, wo Dependency Injection eingesetzt wird.
- **Geschäftsregeln einmal pflegen:** Dieselbe fachliche Regel an einer maßgeblichen Stelle implementieren. Ähnlich aussehender Code allein rechtfertigt noch keine gemeinsame Abstraktion.
- **Absicht verständlich machen:** Verständliche Namen und einen klaren Ablauf bevorzugen. Kommentare erklären Gründe, Randbedingungen und bewusste Abweichungen; veraltete Kommentare und auskommentierten Code entfernen. Vorhandenen Stil bewahren.
- **Fehler strukturiert behandeln:** Fehlerpfade und ungültige Eingaben bewusst berücksichtigen; strukturierte Fehler mit ausreichendem Kontext für die Behandlung bevorzugen.
- **Seiteneffekte kontrollieren:** Fachliche Berechnungen nach Möglichkeit von Ein-/Ausgabe trennen. Veränderlichen Zustand und versteckte Abhängigkeiten auf Uhrzeit, Zufall oder globale Daten begrenzen.
- **Testbarkeit fördern:** Verhalten über klare Schnittstellen prüfbar machen. Tests auf beobachtbare Ergebnisse ausrichten; konkrete Testanforderungen stehen in [[02-Standards/Testing/testing-standards]].
- **Einfachheit bewahren:** Nur benötigte Funktionalität implementieren. Abstraktionen durch konkrete Anforderungen oder nachgewiesene gemeinsame Regeln begründen.
- **Schrittweise refaktorieren:** Struktur in kleinen, nachvollziehbaren Schritten verbessern und das bestehende Verhalten erhalten. Die betroffenen Verhaltensweisen mit passenden Tests absichern; fehlende Absicherung vor riskanten Umbauten ergänzen.

## Projektentscheidungen
Formatierung, statische Analyse und zulässige Komplexität im Projekt festlegen. Keine pauschalen Methodenlängen, Parametergrenzen oder Pflicht zu vollständiger Testabdeckung; Prüfziele nach Risiko und Projektbedingungen festlegen. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].

Dabei insbesondere prüfen: Sind Namen fachlich verständlich, Verantwortlichkeiten klar und Abhängigkeiten nötig? Werden Geschäftsregeln konsistent gepflegt, Fehler und Seiteneffekte sichtbar behandelt und relevante Verhaltensweisen durch Tests abgesichert? Sind zusätzliche Abstraktionen durch den tatsächlichen Bedarf begründet?
