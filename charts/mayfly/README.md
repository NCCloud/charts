# mayfly

![Version: 1.2.0](https://img.shields.io/badge/Version-1.2.0-informational?style=flat-square)

Mayfly Operator

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| goncalolourenco | <goncalo.lourenco@namecheap.com> |  |
| yunussandikci | <yunus.sandikci@namecheap.com> |  |

## Source Code

* <https://github.com/NCCloud/mayfly>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| clusterRole.rules[0].apiGroups[0] | string | `"*"` |  |
| clusterRole.rules[0].resources[0] | string | `"*"` |  |
| clusterRole.rules[0].verbs[0] | string | `"get"` |  |
| clusterRole.rules[0].verbs[1] | string | `"list"` |  |
| clusterRole.rules[0].verbs[2] | string | `"watch"` |  |
| clusterRole.rules[0].verbs[3] | string | `"create"` |  |
| clusterRole.rules[0].verbs[4] | string | `"update"` |  |
| clusterRole.rules[0].verbs[5] | string | `"patch"` |  |
| clusterRole.rules[0].verbs[6] | string | `"delete"` |  |
| dockerconfigjson | string | `""` |  |
| env | list | `[]` |  |
| image.repository | string | `"ghcr.io/nccloud/mayfly"` |  |
| image.tag | string | `"latest"` |  |
| imagePullSecretName | string | `""` |  |
| prometheusRule.additionalLabels | object | `{}` |  |
| prometheusRule.additionalRules | list | `[]` |  |
| prometheusRule.builtInRules.down | bool | `true` |  |
| prometheusRule.builtInRules.reconcileErrors | bool | `true` |  |
| prometheusRule.enabled | bool | `false` |  |
| replicas | int | `1` |  |
| resources.limits.cpu | string | `"500m"` |  |
| resources.limits.memory | string | `"2Gi"` |  |
| resources.requests.cpu | string | `"250m"` |  |
| resources.requests.memory | string | `"1Gi"` |  |
| serviceMonitor.additionalLabels | object | `{}` |  |
| serviceMonitor.enabled | bool | `false` |  |
| tolerations | list | `[]` |  |

