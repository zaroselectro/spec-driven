---
type: placeholder
tags: [projekt, adr]
---

# ADR – _Example-Project

**Bewusst vorgesehener Bereich für spätere Inhalte.** Hier liegen die Architektur-Entscheidungen des Projekts – aktuell keine, weil das Demonstrationsprojekt keine echten Entscheidungen getroffen hat.

## Ablage und Benennung

- Ablage: dieser Ordner, Dateiname nach Muster `ADR-NNN-kurztitel.md` (Nummer fortlaufend).
- Vorlage: [[07-Templates/adr-template]] mit den Pflichtabschnitten Problem, Optionen, Entscheidung, Konsequenzen, Status.
- Statuswerte: `vorgeschlagen` → `akzeptiert` → ggf. `ersetzt`/`obsolet`.

## Regeln

- **MUSS** Nur akzeptierte ADRs gelten als verbindliche Entscheidung; `vorgeschlagen` verpflichtet noch nicht.
- **MUSS** Akzeptierte ADRs werden nicht still überschrieben – eine neue Entscheidung ersetzt sie ausdrücklich und verweist auf die alte.
- **MUSS** Abweichungen von **MUSS**-Regeln aus [[02-Standards/README]] werden hier begründet (Geltungsbereich, Laufzeit, Rückkehrbedingung).
- **SOLLTE** Ein ADR behandelt genau eine Entscheidung; bei mehreren Entscheidungen entstehen mehrere ADRs.
- **SOLLTE** Reihenfolge im Registrierungsfluss einhalten: Vorschlag → Prüfung (Architektur, Security, DevOps) → Freigabe durch den Auftraggeber.

## Ablauf

[[04-Workflows/architecture-design]] · Vorlage: [[07-Templates/adr-template]] · Zuarbeit: [[01-Agents/software-architect]]

**Siehe auch:** [[03-Projects/_Example-Project/Architecture/arc42/README|arc42-Ordner]] · [[03-Projects/_Example-Project/README]]
