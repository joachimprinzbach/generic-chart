# Generic Helm Chart

This chart is a template for common Kubernetes resource manifests, which should cover most use cases. Please read through the list of possible configuration parameters. If you miss a specific feature, you can easily add it via a pull request. If you don't think you can do that, just create a GitHub issue.

## Configuration

| Parameter | Description | Default |
|----------:|:------------|:--------|
| **replicaCount** | Amount of `Pod` replicas | `1` |
| **revisionHistoryLimit** | Amount of old `ReplicaSets` for this `Deployment` should be retained | `1` |
| **image.repository** | URL to the container registry with organisation and repository | `nil` |
| **image.tag** | Image tag of the provided container repository | `nil` |
| **image.pullPolicy** | The pull policy when a image should be pulled (`IfNotPresent`, `Always`) | `IfNotPresent` |
| **imagePullSecrets** | Reference a `Secret` which should be use to authenticate against a container registry | `nil` |
| **nameOverride** | Override the fullname with this name | "" |
| **serviceAccount.create** | If a `ServiceAccount` should be created. If `false` a `ServiceAccount` must be provided and configured correctly with its name under `serviceAccount.name`.  | `true` |
| **serviceAccount.name** | Name of the `ServiceAccount`. If not set and create is true, a name is generated using the name template | `nil` |
| **serviceAccount.automountServiceAccountToken** | If `true` the `Secret` with the `Token` and `Certificates`  of the `ServiceAccount` is mounted. Only required when access to the master API is necessary | `false` |
| **serviceAccount.annotations** | Sets [`annotations`](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for the `ServiceAccount` | `{}` |
| **network** | Map of ports which should be exposed. Adds `ports` section to the Pod template, adds `ports` section to Service and can create `Ingress` or `Route` for the ports. | `network.http.servicePort: 8080` |
| **network.{}.servicePort** | Port number of the `Service` (e.g. 8080, 8443). If `nil` no port on the `Service` is exposed | `nil` |
| **network.{}.containerPort** | The port which is exposed on the `Pod`. If `nil` corresponds to the `network.{}.servicePort` | `nil` |
| **network.{}.ingress** | If not `nil` creates an `Ingress` or `Route` for the `Service` and its `servicePort`. If set to `{}` see `ingress.zone` | `nil` |
| **network.{}.ingress.host** | Sets the hostname for the `Ingress` or `Route`. If `nil` see `ingress.zone` | `nil` |
| **network.{}.ingress.annotations** | Sets [`annotations`](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for the `Ingress` or `Route` instance | `{}` |
| **network.{}.ingress.path** | Sets the path for the `Ingress` or `Route` instance | `/` |
| **service.type** | `Service` type (`ClusterIP`, `NodePort`, `ExternalName`) | `ClusterIP` |
| **service.annotations** | Sets [`annotations`](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for the `Service` | `{}` |
| **ingress.controller** | Sets the type of the ingress controller (e.g. Route, Ingress) | `Route` |
| **env** | List of environment variables for the `Deployment` | `[]` |
| **deployment.strategy** | Specifies the [`strategy`](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy) used to replace old Pods by new ones | `type: RollingUpdate` |
| **envFrom** | Set environment variables from a `ConfigMap` or `Secret`. See [`envFrom`](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#configure-all-key-value-pairs-in-a-configmap-as-container-environment-variables) | `nil` |
| **persistence.enabled** | If `true` a [`PVC`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) is created | `false` |
| **persistence.name** | The name of the PVC | `generic-chart.name` |
| **persistence.accessModes** | [`accessModes`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) of the PVC (ReadWriteOnce, ReadWriteMany) | `ReadWriteOnce` |
| **persistence.storageClass** | [`storageClass`] of the PVC | `` |
| **persistence.size** | Size of the PVC (e.g. 512Mi, 10Gi, 1Ti) | `nil` |
| **persistence.volumeMountPath** | Path where to volume should be mounted (e.g. `/var/data/`). If set, `volumes` and `volumeMounts` are configured | `nil` |
| **persistence.annotations** | Sets [`annotations`](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for the `PersistentVolumeClaim` | `{}` |
| **volumes** | Set [`Volumes`](https://kubernetes.io/docs/concepts/storage/volumes/) available to the `Pod` | `[]` |
| **volumeMounts** | Mounts a [`Volume`](https://kubernetes.io/docs/concepts/storage/volumes/) defined in `volumes` in the container. | `[]` |
| **readinessProbe** | Defines the [`readinessProbe`](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) | `{}` |
| **livenessProbe** | Defines the [`livenessProbe`](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) | `{}` |
| **resources** | CPU/Memory resource [`requests/limits`](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container) | `{}` |
|**podSecurityContext** | [`securityContext`](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) of the `Pod` | `{}`|
|**securityContext** | [`securityContext`](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for the container | `{}`|
|**nodeSelector** | [`nodeSelector`](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector) schedules Pods only on matching nodes | `{}` |
|**tolerations** | [`tolerations`](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) allows to schedule `Pods` on nodes with [`taints`](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)  | `{}` |
|**affinity** | Set [`affinity`](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#node-affinity-beta-feature) to control how pods are scheduled | `{}` |
|**defaultAffinityRules.enabled** | If `true` prevents that the `Pod` defined in `replicaCount` are not scheduled on the same node | `true` |
|**annotations** | Sets [`annotations`](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) for the `Pod` | `{}` |
|**labels** | Sets [`labels`](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) for the `Pod` | `{}` |
|**command** | Sets [`command`](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#define-a-command-and-arguments-when-you-create-a-pod) for the `Pod`. | `[]` |
|**args** | Sets [`args`](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#define-a-command-and-arguments-when-you-create-a-pod) for the `Pod`. | `[]` |
|**initContainers** | Sets [`initContainers`](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) for the `Pod`. | `[]` |
|**additionalContainers** | Define additional containers for the `Pod`. | `[]` |

## Contributions
If you contribute new featuers or fix a bug, please update the `.version` in the `Chart.yaml` according to [SemVer](https://semver.org/) and update the documentation.