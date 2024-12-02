# metadata-reflector

![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-informational?style=flat-square) ![AppVersion: 0.1.0](https://img.shields.io/badge/AppVersion-0.1.0-informational?style=flat-square)

Metadata Reflector

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| olegneychev | <oleg.neychev@namecheap.com> |  |

## Source Code

* <https://github.com/NCCloud/metadata-reflector>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| annotations | object | `{}` |  |
| configuration.backgroundReflectionInterval | string | `"5m"` | The frequency of the background reconciliation. Set to 0 to disable |
| configuration.gomaxprocsOverride | string | `""` | The value for GOMAXPROCS. By default, the CPU limit of the deployment. See https://pkg.go.dev/runtime#hdr-Environment_Variables |
| configuration.gomemlimitOverride | string | `""` | The value for GOMEMLIMIT. By default, the memory limit of the deployment. See https://pkg.go.dev/runtime#hdr-Environment_Variables |
| configuration.namespaces | list | `[]` | A list of namespaces to watch |
| configuration.resourceSelector | object | `{}` | Configure what resources will be watched by the controller. An example can be seen in `values.yaml`. At the moment, only Deployment is supported |
| extraEnvs | list | `[]` | Extra environment variables to be passed to the controller deployment |
| fullnameOverride | string | `"metadata-reflector"` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ghcr.io/nccloud/metadata-reflector"` |  |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion. |
| livenessProbe.failureThreshold | int | `3` |  |
| livenessProbe.httpGet.path | string | `"/healthz"` |  |
| livenessProbe.httpGet.port | int | `8083` |  |
| livenessProbe.httpGet.scheme | string | `"HTTP"` |  |
| livenessProbe.initialDelaySeconds | int | `15` |  |
| livenessProbe.periodSeconds | int | `10` |  |
| livenessProbe.successThreshold | int | `1` |  |
| livenessProbe.timeoutSeconds | int | `1` |  |
| metrics.port | int | `9090` | The port to expose Prometheus metrics on |
| metrics.serviceMonitor | object | `{"annotations":{},"enabled":false,"path":"/metrics","scrapeInterval":"1m","scrapeTimeout":"10s"}` | Service Monitor configuration. Enable if Prometheus is installed in your cluster |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| readinessProbe.failureThreshold | int | `3` |  |
| readinessProbe.httpGet.path | string | `"/readyz"` |  |
| readinessProbe.httpGet.port | int | `8083` |  |
| readinessProbe.httpGet.scheme | string | `"HTTP"` |  |
| readinessProbe.initialDelaySeconds | int | `5` |  |
| readinessProbe.periodSeconds | int | `10` |  |
| readinessProbe.successThreshold | int | `1` |  |
| readinessProbe.timeoutSeconds | int | `1` |  |
| replicaCount | int | `1` | The number of controller replicas to run. Leader election is enabled for 2 and more replicas |
| resources.limits.memory | string | `"512Mi"` |  |
| resources.requests.cpu | string | `"50m"` |  |
| resources.requests.memory | string | `"128Mi"` |  |
| securityContext.runAsNonRoot | bool | `true` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.automount | bool | `true` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `"metadata-reflector"` | The name of the service account to use. |
| tolerations | list | `[]` |  |
