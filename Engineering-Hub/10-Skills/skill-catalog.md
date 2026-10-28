# Skills-Katalog

Die Skill-Namen in der letzten Spalte öffnen die vollständigen Beschreibungen als sichtbare Obsidian-Notizen unter `10-Skills`. Die Agent-Pakete liegen zusätzlich unter `.agents/skills`.

Die Methoden sind unabhängig von einem bestimmten Agent-Produkt nutzbar. „Paket vorhanden“ bedeutet: Eine `SKILL.md` liegt im Repository; es bedeutet keine nachgewiesene Aktivierung in jeder Sitzung.

| Fähigkeit | Einsatz / Eingaben | Ergebnis und Qualitätsmaßstab | Passende Rollen | Agent-Paket |
| --- | --- | --- | --- | --- |
| Anforderungsanalyse | Idee, Zielgruppe, vorhandene Anforderungen | Abgegrenzte Anforderungen, prüfbare Akzeptanzkriterien, offene Fragen; Annahmen sichtbar | [[01-Agents/requirements-analyst]], [[01-Agents/software-architect]] | [[10-Skills/Requirements/hub-requirements-analysis\|hub-requirements-analysis]] |
| Code-Review | Festgelegter Diff, gewünschtes Verhalten, geltende Regeln | Priorisierte, belegte Befunde mit Fundstelle und Auswirkung; Prüflücken sichtbar | [[01-Agents/qa-engineer]], [[01-Agents/security-reviewer]], Entwicklerrollen | [[10-Skills/Quality/hub-code-review\|hub-code-review]] |
| Spezifikation implementieren | Benannte Spec, Akzeptanzkriterien, Zielprojekt | Code und Tests nach aktiven Regeln; Implementierungsplan, Review und Soll-Ist-Abgleich je Kriterium | Entwicklerrollen, [[01-Agents/qa-engineer]] | [[10-Skills/Development/implement-from-spec\|implement-from-spec]] |
| Architekturentscheidungen dokumentieren | Problem, Alternativen, Randbedingungen | ADR mit Entscheidung, Begründung und Konsequenzen | [[01-Agents/software-architect]] | Konzept; [[04-Workflows/architecture-design]], [[07-Templates/adr-template]] |
| Tests entwerfen | Akzeptanzkriterien, Risiken, bestehende Tests | Prüffälle für Haupt-, Fehler- und relevante Randfälle | [[01-Agents/qa-engineer]] | Konzept; [[02-Standards/Testing/testing-standards]] |
| API dokumentieren | Tatsächlicher API-Vertrag, Implementierung | Konsistente Requests, Responses, Fehlerfälle und Beispiele | [[01-Agents/backend-developer]], [[01-Agents/frontend-developer]] | Konzept; [[02-Standards/API/rest-standards]] |

## Beispiel: Anforderungsanalyse als Methode

**Auslöser:** Eine Produktidee soll umsetzbar und prüfbar beschrieben werden. **Eingaben:** Nutzerziel, vorhandene Fachregeln und bekannte Einschränkungen. **Vorgehen:** Umfang abgrenzen, beobachtbares Verhalten beschreiben, Akzeptanzkriterien ableiten und Wissenslücken kennzeichnen. **Ergebnis:** Feature-Beschreibung gemäß [[07-Templates/feature-template]], soweit der Auftrag das erfordert.

**Qualität:** Jedes Kriterium lässt sich überprüfen; Annahmen sind keine bestätigten Anforderungen. Architektur und Technologien werden nur festgelegt, wenn sie bereits vorgegeben oder ausdrücklich Teil des Auftrags sind.

Die technische Umsetzung dieser Methode steht im verlinkten Agent-Paket. Weitere Methoden können mit [[07-Templates/skill-profile-template]] ausgearbeitet werden.
