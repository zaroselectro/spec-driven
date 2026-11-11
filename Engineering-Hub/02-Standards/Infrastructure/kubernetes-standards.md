# Kubernetes und Helm

## Geltungsbereich
Gilt für Änderungen am jeweiligen technischen Bereich; andere Standards nur bei Berührung hinzunehmen. Zielstände sind Vorgaben, keine aktuelle Supportbewertung.

## Verbindliche Regeln (MUSS)
Secrets nicht als Klartext in versionierten Values speichern. RBAC auf benötigte Rechte begrenzen. Rollout und Rollback dokumentieren; produktive Änderungen explizit freigeben lassen.

Die [[02-Standards/General/cloud-native-standards|Cloud-native Engineering Standards]] einhalten; sie regeln insbesondere Ressourcen, Health/Readiness, Observability und Graceful Shutdown verbindlich. Probes, Ressourcenwerte und Beendigungsfristen in den Workload-Manifesten passend zur Anwendung konfigurieren.

## Empfehlungen (SOLLTE)
Bei langen Startphasen eine passende Startup-Probe vorsehen. Alarmierung definieren. Helm-Templates vor Deployment rendern und validieren.

## Projektentscheidungen
Namespaces, Netzwerkrichtlinien, Chart-Versionen, Verfügbarkeit, Persistenz und Secret-Verwaltung bestimmen. Rollback von Datenmigrationen gesondert bewerten. Abweichungen begründet mit Status und Geltungsbereich in einem Projekt-ADR festhalten.

## Prüfung
Im Review Anforderungen, Änderung und Testnachweise gegen diese Regeln prüfen. Offene Entscheidungen als offen markieren. Siehe [[04-Workflows/code-review]].
