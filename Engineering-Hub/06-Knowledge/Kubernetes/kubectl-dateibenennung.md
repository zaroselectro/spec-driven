---
type: knowledge
topic: kubernetes
tags: [wissen, kubernetes, kubectl, manifests]
---

# Spec: Dateibenennung und Nummerierung von Kubernetes-Manifesten

Stand: 2026-10-25. Diese Spec beschreibt eine empfohlene Projektkonvention für YAML-Manifeste, die mit `kubectl` verarbeitet werden. Sie ist Erfahrungswissen, kein verbindlicher Kubernetes-Standard. Verbindliche Betriebsregeln stehen in [[02-Standards/Infrastructure/kubernetes-standards]].

## Geltungsbereich

Mit „kubectl-Dateien“ sind hier Kubernetes-Ressourcenmanifeste gemeint, beispielsweise für Deployments und Services. Kubernetes legt weder deren Basisnamen noch numerische Präfixe fest. `kubectl apply -f` verarbeitet YAML und JSON; für diese Konvention verwenden wir einheitlich `.yaml`.

Die lokale Zugriffsdatei `~/.kube/config` (kubeconfig), Shell-Skripte und Helm-Templates gehören nicht zu diesem Nummerierungsschema. Bei Kustomize bleibt der Steuerdateiname `kustomization.yaml` ohne Nummer; die referenzierten Ressourcen können dem Schema folgen.

## Namensschema

```text
<nummer>-<ressourcentyp>-<name>.yaml
```

- `nummer`: zweistellig, mit führender Null, aus der Tabelle unten.
- `ressourcentyp`: `kind` in Kleinbuchstaben, etwa `serviceaccount`, `configmap` oder `deployment`.
- `name`: der tatsächliche Wert aus `metadata.name`, vorzugsweise in Kleinbuchstaben mit Bindestrichen. Die zulässigen Kubernetes-Namen richten sich weiterhin nach dem jeweiligen Ressourcentyp.
- Pro Datei wird üblicherweise eine Ressource beschrieben. Mehrere Ressourcen desselben Typs erhalten dieselbe Nummer und unterschiedliche Namen; die Nummer ist eine Gruppe, keine eindeutige Datei-ID.
- Gleiche Ressourcen behalten in getrennten Umgebungsverzeichnissen denselben Dateinamen. Die Umgebung wird über das Verzeichnis unterschieden, beispielsweise `k8s/dev/` und `k8s/prod/`.

Beispiel: `60-deployment-orders-api.yaml` beschreibt ein `Deployment` mit `metadata.name: orders-api`. Der Dateiname setzt weder den Ressourcennamen noch den Namespace; dafür sind die Manifestfelder maßgeblich.

## Standardnummern dieser Konvention

| Nummer | Ressourcentyp / Zweck | Beispiel |
| --- | --- | --- |
| `00` | Namespace | `00-namespace-orders.yaml` |
| `05` | CustomResourceDefinition (optionale Cluster-Voraussetzung) | `05-customresourcedefinition-widgets.example.com.yaml` |
| `10` | ServiceAccount | `10-serviceaccount-orders-api.yaml` |
| `15` | Role / ClusterRole | `15-role-orders-reader.yaml` |
| `20` | RoleBinding / ClusterRoleBinding | `20-rolebinding-orders-reader.yaml` |
| `25` | ResourceQuota / LimitRange | `25-resourcequota-orders.yaml` |
| `30` | ConfigMap | `30-configmap-orders-api.yaml` |
| `35` | Secret bzw. projektspezifische Secret-Referenz | `35-secret-orders-api.yaml` |
| `40` | PersistentVolume (optionale Cluster-Voraussetzung) | `40-persistentvolume-orders-data.yaml` |
| `45` | PersistentVolumeClaim | `45-persistentvolumeclaim-orders-data.yaml` |
| `50` | Service | `50-service-orders-api.yaml` |
| `55` | NetworkPolicy | `55-networkpolicy-orders-api.yaml` |
| `60` | Deployment | `60-deployment-orders-api.yaml` |
| `65` | StatefulSet / DaemonSet | `65-statefulset-orders-db.yaml` |
| `70` | Job | `70-job-orders-migration.yaml` |
| `75` | CronJob | `75-cronjob-orders-cleanup.yaml` |
| `80` | HorizontalPodAutoscaler | `80-horizontalpodautoscaler-orders-api.yaml` |
| `85` | PodDisruptionBudget | `85-poddisruptionbudget-orders-api.yaml` |
| `90` | Ingress | `90-ingress-orders-api.yaml` |
| `95` | Weitere Ressourcen, z. B. Custom Resources | `95-widget-orders.yaml` |

Nur benötigte Ressourcen werden angelegt; ausgelassene Nummern bleiben frei. Zusätzliche Typen und Abweichungen werden im Projekt dokumentiert. Clusterweite Ressourcen werden getrennt von Anwendungsmanifesten verwaltet, wenn Zuständigkeiten oder Berechtigungen dies verlangen. Die Secret-Zeile benennt nur die Dateigruppe: Sie erlaubt keine versionierten Klartext-Geheimnisse. Bei Secret-Operatoren richtet sich der Typ im Dateinamen nach dem tatsächlichen `kind`.

## Nummerierung und Ausführung

Die Nummern erleichtern das Sortieren und Lesen. Sie sind kein Abhängigkeitsmanager und keine Garantie für Betriebsbereitschaft. Auch ein erfolgreiches `apply` bedeutet nicht, dass Pods bereit oder Jobs abgeschlossen sind.

```sh
# Einzelnes Manifest prüfen, ohne es im Cluster zu speichern
kubectl apply --dry-run=client -f k8s/dev/60-deployment-orders-api.yaml

# Geplante Änderungen einer Umgebung anzeigen
kubectl diff -f k8s/dev/

# Manifeste einer Umgebung anwenden
kubectl apply -f k8s/dev/
```

Diese Befehle sind Anwendungsbeispiele; produktive Änderungen unterliegen weiterhin den Freigaberegeln. Client-Dry-Run ersetzt keine serverseitige Validierung. `kubectl diff` benötigt Clusterzugriff und liefert bei Unterschieden regulär Exit-Code `1`.

Echte Abhängigkeiten werden im Auslieferungsablauf explizit behandelt:

- Namespace vor den zugehörigen Ressourcen anlegen.
- CRDs zuerst anwenden und auf `Established` warten, bevor Custom Resources angelegt werden; benötigte Controller müssen ebenfalls verfügbar sein.
- Eine erforderliche Datenmigration vor dem davon abhängigen Deployment ausführen und ihren erfolgreichen Abschluss abwarten. `70` bedeutet nicht, dass eine Migration nach dem Deployment laufen soll.
- Auf benötigte Rollouts ausdrücklich warten, beispielsweise mit `kubectl rollout status deployment/orders-api -n orders`.

Für solche Phasen einzelne Dateien oder getrennte Verzeichnisse gezielt anwenden. Ein pauschales `kubectl apply -f <verzeichnis>` ersetzt diese Steuerung nicht. Bei Kustomize wird die Umgebung mit `kubectl apply -k <verzeichnis>` angewendet.

## Prüfpunkte für die Übernahme im Projekt

- Dateinamen folgen dem Schema und der Typ entspricht dem tatsächlichen `kind`.
- Die Nummer passt zur Tabelle; zusätzliche Typen oder Abweichungen sind dokumentiert.
- Dateiname und `metadata.name` stimmen überein; Namespace und Umgebung sind eindeutig zugeordnet.
- Geheimnisse werden über die im Projekt festgelegte sichere Verwaltung eingebunden.
- Abhängigkeiten und Wartebedingungen sind im Auslieferungsablauf beschrieben.

## Quellen und Einordnung

Die Nummerntabelle ist ein Vorschlag dieser Spec, keine Vorgabe der folgenden Quellen:

- [Kubernetes: kubectl apply](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_apply/) – Dateieingabe, Formate, Dry-Run und Kustomize.
- [Kubernetes: deklarative Objektverwaltung](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/) – Verzeichnisverarbeitung und `kubectl diff`.
- [Kubernetes: Managing Workloads](https://kubernetes.io/docs/concepts/workloads/management/) – mehrere Manifeste und Reihenfolge innerhalb eines Manifests.

**Siehe auch:** [[06-Knowledge/Kubernetes/README]]
