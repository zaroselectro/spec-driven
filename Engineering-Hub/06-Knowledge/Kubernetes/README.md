---
type: knowledge
topic: kubernetes
tags: [wissen, kubernetes]
---

# Wissen: Kubernetes

Hintergrund zur Laufzeitumgebung. **Verbindlich** ist ausschließlich [[02-Standards/Infrastructure/kubernetes-standards]].

## Weiterführende Specs

- [[06-Knowledge/Kubernetes/kubectl-dateibenennung|Dateibenennung und Nummerierung von Kubernetes-Manifesten]]: empfohlenes Namensschema, Standardnummern pro Ressourcentyp und Umgang mit Ausführungsabhängigkeiten.

## Grundmodell in Kurzform

- **Deklarativer Zustand:** Man beschreibt den gewünschten Zustand; die Steuerung bringt die realen Objekte in diesen Zustand (Reconciliation).
- **Pod** = eine oder mehrere eng gekoppelte Container mit gemeinsamer Netzadresse; die baueinheitliche Skalierungseinheit ist der Pod.
- **Deployment** verwaltet Replikate und steuert Rollouts; **Service** stellt einen stabilen Namen bereit; **Ingress** regelt den externen Zugang.
- **ConfigMap / Secret:** Konfiguration und Geheimnisse getrennt von Abbild und Manifest (siehe [[02-Standards/General/security-principles]]).
- **Namespace** gliedert Ressourcen und Rechte innerhalb eines Clusters – er ist keine Sicherheitsgrenze von allein.

## Warum Requests und Limits Pflicht sind

Ohne Anforderungen konkurrieren Pods um Ressourcen nach Zufallsprinzip; ein überlasteter Knoten trifft zufällig. Requests ermöglichen Planung, Limits begrenzen Ausreißer. Fehlende Werte führen zu schwer vorhersagbaren Störungen – deshalb **MUSS** in den Standards.

## Probes und Auslieferung

- **Readiness** entscheidet, ob Anfragen weitergereicht werden (z. B. noch migrierend, noch nicht verbunden).
- **Liveness** entscheidet, ob neu gestartet werden soll (nur bei echter Sackgasse – sonst Restart-Schleifen).
- Rollouts erfolgen schrittweise; **vorher** muss der Rollback-Weg beschrieben sein, nicht danach.

## Häufige Fallstricke

| Stolperstein | Folge |
| --- | --- |
| `latest` als Bildversion | unbekannter Zustand, kein belastbarer Rollback |
| Geheimnisse im Manifest/Values-Datei | Leak im Repositoryverlauf |
| Kein Limit, kein Request | unberechenbare Störungen unter Last |
| Logs nur in Containerdateien | Diagnose erst nach Pod-Ende möglich |
| Restart ohne Readiness-Logik | Anfragen laufen in eine noch nicht bereite Anwendung |

## Zusammenhang mit den anderen Bereichen

Bild: [[02-Standards/Infrastructure/docker-standards]] · Anwendung (Gesundheit, Konfiguration): [[02-Standards/Java/spring-boot-standards]] · Rolle: [[01-Agents/devops-engineer]]

**Siehe auch:** [[06-Knowledge/README]]
