# Helm chart for Unifi Network Application

[Unifi Network Application](https://ui.com/download), is a tool to manage Unifi Network devices.

## TL;DR

```console
$ helm install my-release oci://registry-1.docker.io/captnbp/unifi-network-application
```

## Prerequisites

- Kubernetes 1.30+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure
- [cert-manager](https://cert-manager.io/)
- MongoDB instance

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install my-release oci://registry-1.docker.io/captnbp/unifi-network-application
```

These commands deploy unifi-network-application on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` release:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release. Remove also the chart using `--purge` option:

```console
$ helm delete --purge my-release
```


## Parameters

### Global parameters

| Name                      | Description                                     | Value |
| ------------------------- | ----------------------------------------------- | ----- |
| `global.imageRegistry`    | Global Docker image registry                    | `""`  |
| `global.imagePullSecrets` | Global Docker registry secret names as an array | `[]`  |
| `global.storageClass`     | Global StorageClass for Persistent Volume(s)    | `""`  |

### Common parameters

| Name                     | Description                                                                                  | Value           |
| ------------------------ | -------------------------------------------------------------------------------------------- | --------------- |
| `nameOverride`           | String to partially override common.names.fullname template (will maintain the release name) | `""`            |
| `fullnameOverride`       | String to fully override common.names.fullname template                                      | `""`            |
| `commonLabels`           | Labels to add to all deployed objects                                                        | `{}`            |
| `commonAnnotations`      | Annotations to add to all deployed objects                                                   | `{}`            |
| `kubeVersion`            | Force target Kubernetes version (using Helm capabilities if not set)                         | `""`            |
| `clusterDomain`          | Default Kubernetes cluster domain                                                            | `cluster.local` |
| `extraDeploy`            | Array of extra objects to deploy with the release                                            | `[]`            |
| `diagnosticMode.enabled` | Enable diagnostic mode (all probes will be disabled and the command will be overridden)      | `false`         |
| `diagnosticMode.command` | Command to override all containers in the chart release                                      | `["sleep"]`     |
| `diagnosticMode.args`    | Args to override all containers in the chart release                                         | `["infinity"]`  |

### unifi-network-application parameters

| Name                 | Description                                                                 | Value          |
| -------------------- | --------------------------------------------------------------------------- | -------------- |
| `image.registry`     | unifi-network-application image registry                                                      | `docker.io`    |
| `image.repository`   | unifi-network-application image repository                                                    | `""`           |
| `image.tag`          | unifi-network-application image tag (immutable tags are recommended)                          | `1.21.0`       |
| `image.pullPolicy`   | Image pull policy                                                           | `IfNotPresent` |
| `image.pullSecrets`  | Specify docker-registry secret names as an array                            | `[]`           |
| `image.debug`        | Specify if debug logs should be enabled                                     | `false`        |
| `extraEnvVars`       | Extra environment variables to be set on unifi-network-application container                  | `{}`           |
| `extraEnvVarsCM`     | ConfigMap with extra environment variables                                  | `""`           |
| `extraEnvVarsSecret` | Secret with extra environment variables                                     | `""`           |
| `command`            | Default container command (useful when using custom images). Use array form | `[]`           |
| `args`               | Default container args (useful when using custom images). Use array form    | `[]`           |

### unifi-network-application deployment/statefulset parameters

| Name                                                | Description                                                                                              | Value                         |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `schedulerName`                                     | Specifies the schedulerName, if it's nil uses kube-scheduler                                             | `""`                          |
| `updateStrategy.type`                               | unifi-network-applicationwarden statefulset strategy type                                                                  | `RollingUpdate`               |
| `updateStrategy.rollingUpdate`                      | unifi-network-applicationwarden statefulset rolling update configuration parameters                                        | `{}`                          |
| `hostAliases`                                       | unifi-network-application pod host aliases                                                                                 | `[]`                          |
| `containerPorts.http`                               | unifi-network-application container port to open for unifi-network-application http                                                          | `8081`                        |
| `containerPorts.https`                              | unifi-network-application container port to open for unifi-network-application https                                                         | `9898`                        |
| `podSecurityContext.enabled`                        | Enable pod Security Context                                                                              | `true`                        |
| `podSecurityContext.fsGroup`                        | Group ID for the container                                                                               | `1001`                        |
| `podSecurityContext.seccompProfile.type`            | Type of seccomp profile to use                                                                           | `RuntimeDefault`              |
| `containerSecurityContext.enabled`                  | Enable container Security Context                                                                        | `true`                        |
| `containerSecurityContext.runAsUser`                | User ID for the container                                                                                | `0`                           |
| `containerSecurityContext.runAsNonRoot`             | Avoid running as root User                                                                               | `false`                       |
| `containerSecurityContext.allowPrivilegeEscalation` | Allow privilege escalation                                                                               | `true`                        |
| `containerSecurityContext.readOnlyRootFilesystem`   | Read-only root filesystem                                                                                | `false`                       |
| `containerSecurityContext.capabilities.drop`        | Capabilities to drop                                                                                     | `["ALL"]`                     |
| `containerSecurityContext.capabilities.add`         | Capabilities to add                                                                                      | `["CHOWN","SETGID","SETUID"]` |
| `podLabels`                                         | Extra labels for unifi-network-application pods                                                                            | `{}`                          |
| `podAnnotations`                                    | Annotations for unifi-network-application pods                                                                             | `{}`                          |
| `podAffinityPreset`                                 | Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`                      | `""`                          |
| `podAntiAffinityPreset`                             | Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`                 | `soft`                        |
| `nodeAffinityPreset.type`                           | Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`                | `""`                          |
| `nodeAffinityPreset.key`                            | Node label key to match. Ignored if `affinity` is set.                                                   | `""`                          |
| `nodeAffinityPreset.values`                         | Node label values to match. Ignored if `affinity` is set.                                                | `[]`                          |
| `affinity`                                          | Affinity for pod assignment. Evaluated as a template.                                                    | `{}`                          |
| `nodeSelector`                                      | Node labels for pod assignment. Evaluated as a template.                                                 | `{}`                          |
| `tolerations`                                       | Tolerations for pod assignment. Evaluated as a template.                                                 | `[]`                          |
| `topologySpreadConstraints`                         | Topology Spread Constraints for unifi-network-application pods assignment spread across your cluster among failure-domains | `[]`                          |
| `priorityClassName`                                 | unifi-network-application pods' priorityClassName                                                                          | `""`                          |
| `resources.limits`                                  | The resources limits for the unifi-network-application container                                                           | `{}`                          |
| `resources.requests`                                | The requested resources for the unifi-network-application container                                                        | `{}`                          |
| `livenessProbe.enabled`                             | Enable livenessProbe                                                                                     | `false`                       |
| `livenessProbe.initialDelaySeconds`                 | Initial delay seconds for livenessProbe                                                                  | `5`                           |
| `livenessProbe.periodSeconds`                       | Period seconds for livenessProbe                                                                         | `5`                           |
| `livenessProbe.timeoutSeconds`                      | Timeout seconds for livenessProbe                                                                        | `5`                           |
| `livenessProbe.failureThreshold`                    | Failure threshold for livenessProbe                                                                      | `5`                           |
| `livenessProbe.successThreshold`                    | Success threshold for livenessProbe                                                                      | `1`                           |
| `readinessProbe.enabled`                            | Enable readinessProbe                                                                                    | `true`                        |
| `readinessProbe.initialDelaySeconds`                | Initial delay seconds for readinessProbe                                                                 | `5`                           |
| `readinessProbe.periodSeconds`                      | Period seconds for readinessProbe                                                                        | `5`                           |
| `readinessProbe.timeoutSeconds`                     | Timeout seconds for readinessProbe                                                                       | `1`                           |
| `readinessProbe.failureThreshold`                   | Failure threshold for readinessProbe                                                                     | `5`                           |
| `readinessProbe.successThreshold`                   | Success threshold for readinessProbe                                                                     | `1`                           |
| `startupProbe.enabled`                              | Enable startupProbe                                                                                      | `false`                       |
| `startupProbe.initialDelaySeconds`                  | Initial delay seconds for startupProbe                                                                   | `0`                           |
| `startupProbe.periodSeconds`                        | Period seconds for startupProbe                                                                          | `10`                          |
| `startupProbe.timeoutSeconds`                       | Timeout seconds for startupProbe                                                                         | `5`                           |
| `startupProbe.failureThreshold`                     | Failure threshold for startupProbe                                                                       | `60`                          |
| `startupProbe.successThreshold`                     | Success threshold for startupProbe                                                                       | `1`                           |
| `customLivenessProbe`                               | Override default liveness probe                                                                          | `{}`                          |
| `customReadinessProbe`                              | Override default readiness probe                                                                         | `{}`                          |
| `customStartupProbe`                                | Override default startup probe                                                                           | `{}`                          |
| `lifecycleHooks`                                    | for the unifi-network-application container(s) to automate configuration before or after startup                           | `{}`                          |
| `extraVolumes`                                      | Optionally specify extra list of additional volumes for unifi-network-application pods                                     | `[]`                          |
| `extraVolumeMounts`                                 | Optionally specify extra list of additional volumeMounts for unifi-network-application container(s)                        | `[]`                          |
| `initContainers`                                    | Add additional init containers to the unifi-network-application pods                                                       | `[]`                          |
| `sidecars`                                          | Add additional sidecar containers to the unifi-network-application pods                                                    | `[]`                          |

### Exposure parameters

| Name                               | Description                                                                                                                      | Value                    |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| `service.type`                     | Kubernetes service type                                                                                                          | `ClusterIP`              |
| `service.ports.http`               | unifi-network-application service HTTP port                                                                                                        | `8200`                   |
| `service.ports.https`              | unifi-network-application service HTTPS port                                                                                                       | `8201`                   |
| `service.nodePorts`                | Specify the nodePort values for the LoadBalancer and NodePort service types.                                                     | `{}`                     |
| `service.sessionAffinity`          | Control where client requests go, to the same pod or round-robin                                                                 | `None`                   |
| `service.sessionAffinityConfig`    | Additional settings for the sessionAffinity                                                                                      | `{}`                     |
| `service.clusterIP`                | unifi-network-application service clusterIP IP                                                                                                     | `""`                     |
| `service.loadBalancerIP`           | loadBalancerIP for the SuiteCRM Service (optional, cloud specific)                                                               | `""`                     |
| `service.loadBalancerSourceRanges` | Address that are allowed when service is LoadBalancer                                                                            | `[]`                     |
| `service.externalTrafficPolicy`    | Enable client source IP preservation                                                                                             | `Cluster`                |
| `service.annotations`              | Additional custom annotations for unifi-network-application service                                                                                | `{}`                     |
| `service.extraPorts`               | Extra port to expose on unifi-network-application service                                                                                          | `[]`                     |
| `service.extraHeadlessPorts`       | Extra ports to expose on unifi-network-application headless service                                                                                | `[]`                     |
| `service.ipFamilyPolicy`           | Controller Service ipFamilyPolicy (optional, cloud specific)                                                                     | `PreferDualStack`        |
| `service.ipFamilies`               | Controller Service ipFamilies (optional, cloud specific)                                                                         | `["IPv6","IPv4"]`        |
| `ingress.enabled`                  | Enable ingress record generation for unifi-network-application                                                                                     | `true`                   |
| `ingress.pathType`                 | Ingress path type                                                                                                                | `ImplementationSpecific` |
| `ingress.apiVersion`               | Force Ingress API version (automatically detected if not set)                                                                    | `""`                     |
| `ingress.hostname`                 | Default host for the ingress record                                                                                              | `unifi-network-application.local`          |
| `ingress.ingressClassName`         | IngressClass that will be be used to implement the Ingress (Kubernetes 1.18+)                                                    | `traefik`                |
| `ingress.ingressControllerType`    | ingressControllerType that will be be used to implement the Ingress specific annotations (Ex. nginx or traefik)                  | `traefik`                |
| `ingress.path`                     | Default path for the ingress record                                                                                              | `/`                      |
| `ingress.annotations`              | Additional annotations for the Ingress resource. To enable certificate autogeneration, place here your cert-manager annotations. | `{}`                     |
| `ingress.tls`                      | Enable TLS configuration for the host defined at `ingress.hostname` parameter                                                    | `false`                  |
| `ingress.selfSigned`               | Create a TLS secret for this ingress record using self-signed certificates generated by Helm                                     | `false`                  |
| `ingress.extraHosts`               | An array with additional hostname(s) to be covered with the ingress record                                                       | `[]`                     |
| `ingress.extraPaths`               | An array with additional arbitrary paths that may need to be added to the ingress under the main host                            | `[]`                     |
| `ingress.extraTls`                 | TLS configuration for additional hostname(s) to be covered with this ingress record                                              | `[]`                     |
| `ingress.secrets`                  | Custom TLS certificates as secrets                                                                                               | `[]`                     |
| `ingress.extraRules`               | Additional rules to be covered with this ingress record                                                                          | `[]`                     |

### RBAC parameter

| Name                                          | Description                                                    | Value   |
| --------------------------------------------- | -------------------------------------------------------------- | ------- |
| `serviceAccount.create`                       | Enable the creation of a ServiceAccount for unifi-network-applicationwarden pods | `true`  |
| `serviceAccount.name`                         | Name of the created ServiceAccount                             | `""`    |
| `serviceAccount.automountServiceAccountToken` | Auto-mount the service account token in the pod                | `false` |
| `serviceAccount.annotations`                  | Additional custom annotations for the ServiceAccount           | `{}`    |

### Persistence parameters

| Name                        | Description                                                       | Value               |
| --------------------------- | ----------------------------------------------------------------- | ------------------- |
| `persistence.enabled`       | Enable unifi-network-application data persistence using PVC. If false, use emptyDir | `true`              |
| `persistence.storageClass`  | PVC Storage Class for unifi-network-application data volume                         | `""`                |
| `persistence.mountPath`     | Data volume mount path                                            | `/data`             |
| `persistence.accessModes`   | PVC Access Modes for unifi-network-application data volume                          | `["ReadWriteOnce"]` |
| `persistence.size`          | PVC Storage Request for unifi-network-application data volume                       | `8Gi`               |
| `persistence.annotations`   | Annotations for the PVC                                           | `{}`                |
| `persistence.existingClaim` | Name of an existing PVC to use (only in `standalone` mode)        | `""`                |

### Global TLS settings for internal CA

### Prometheus metrics

### Database parameters


### NetworkPolicy Configuration

## License

[MIT](./LICENSE).

## Author

This Helm chart was created and is being maintained by @captnbp.

### Credits

- The `unifi-network-application` project can be found [here](https://community.ui.com/releases/UniFi-Network-Application-10-5-67/375288b9-a4b4-46f1-a19d-5c787d342c2b)