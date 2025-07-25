# watchtower

![Version: 1.3.1](https://img.shields.io/badge/Version-1.3.1-informational?style=flat-square)

Watchtower

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| yunussandikci | <yunus.sandikci@namecheap.com> |  |

## Source Code

* <https://github.com/NCCloud/watchtower>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| config.enableLeaderElection | string | `"true"` |  |
| config.syncPeriod | string | `"36h"` |  |
| config.watcherRefreshPeriod | string | `"30s"` |  |
| dockerconfigjson | string | `""` |  |
| env | list | `[]` |  |
| extraEnvs | object | `{}` |  |
| image.repository | string | `"ghcr.io/nccloud/watchtower"` |  |
| image.tag | string | `"latest"` |  |
| imagePullSecretName | string | `""` |  |
| prometheusRule.additionalLabels | object | `{}` |  |
| prometheusRule.additionalRules | list | `[]` |  |
| prometheusRule.builtInRules.down | bool | `true` |  |
| prometheusRule.builtInRules.reconcileErrors | bool | `true` |  |
| prometheusRule.enabled | bool | `false` |  |
| replicas | int | `1` |  |
| resources.limits.cpu | string | `"500m"` |  |
| resources.limits.memory | string | `"1Gi"` |  |
| resources.requests.cpu | string | `"200m"` |  |
| resources.requests.memory | string | `"256Mi"` |  |
| serviceMonitor.additionalLabels | object | `{}` |  |
| serviceMonitor.enabled | bool | `false` |  |
| tolerations | list | `[]` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)
