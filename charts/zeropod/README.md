# zeropod

![Version: 0.1.3](https://img.shields.io/badge/Version-0.1.3-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.11.3](https://img.shields.io/badge/AppVersion-v0.11.3-informational?style=flat-square)

Kubernetes runtime for scaling containers to zero after a certain amount of time of the last TCP connection using CRIU checkpointing

This chart installs [zeropod](https://github.com/ctrox/zeropod) on your cluster.

Zeropod intercepts network connections at the eBPF level. When no connections are active for a
configured duration, it checkpoints the container to disk using [CRIU](https://criu.org) and
adjusts its resource requests to zero. The next incoming connection triggers a restore. From the
application's perspective the process is never restarted — its memory and file descriptors are
fully preserved. Depending on the memory size of the checkpointed program this happens in tens
to a few hundred milliseconds, virtually unnoticeable to the user.

## Requirements

- Kubernetes 1.23+ (1.31+ for `inPlaceScaling`)
- Containerd 1.7+
- `runtimeClass` must be referenced in pod specs: `runtimeClassName: zeropod`
- Nodes must be labeled `zeropod.ctrox.dev/node=true` before the DaemonSet will schedule onto them

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity rules for the DaemonSet |
| gke | bool | `false` | Enable GKE-specific host paths (sets zeropodOpt to /var/lib/toolbox/zeropod and passes -host-opt-path to the installer) |
| image.installer.pullPolicy | string | `"IfNotPresent"` | Image pull policy for the installer init container |
| image.installer.repository | string | `"ghcr.io/ctrox/zeropod-installer"` | Repository for the installer init container image |
| image.installer.tag | string | `"v0.11.3"` | Tag for the installer image; defaults to the chart appVersion if not set |
| image.manager.pullPolicy | string | `"IfNotPresent"` | Image pull policy for the manager container |
| image.manager.repository | string | `"ghcr.io/ctrox/zeropod-manager"` | Repository for the manager container image |
| image.manager.tag | string | `"v0.11.3"` | Tag for the manager image; defaults to the chart appVersion if not set |
| image.prepareBpfFs | object | `{"pullPolicy":"IfNotPresent","repository":"alpine","tag":"3.19.1"}` | Alpine image used to mount the eBPF filesystem before the manager starts |
| image.prepareBpfFs.pullPolicy | string | `"IfNotPresent"` | Image pull policy for the prepare-bpf-fs init container |
| image.prepareBpfFs.repository | string | `"alpine"` | Repository for the prepare-bpf-fs init container image |
| image.prepareBpfFs.tag | string | `"3.19.1"` | Tag for the prepare-bpf-fs image |
| installer.criuImage | string | `"ghcr.io/ctrox/zeropod-criu:v4.2"` | CRIU container image pulled and installed by the installer init container |
| installer.uninstall | bool | `false` | Run the installer in uninstall mode to remove all zeropod files from the host |
| k3s | bool | `false` | Enable K3s-specific host paths and pass -runtime=k3s -probe-binary-name=k3s to the installer |
| manager.debug | bool | `false` | Enable debug logging in the manager |
| manager.inPlaceScaling | bool | `true` | Adjust pod resource requests to zero during scale-down; requires the InPlacePodVerticalScaling feature gate |
| manager.metricsAddr | string | `":8080"` | Address for the Prometheus metrics endpoint |
| manager.migrationManager | bool | `true` | Enable CRUD access to Migration CRs and pod listing for node-to-node pod migration |
| manager.podUpdater | bool | `true` | Enable updating pod spec and resize subresource; required when inPlaceScaling or statusLabels is enabled |
| manager.securityContext | object | `{"appArmorProfile":{"type":"Unconfined"},"capabilities":{"add":["NET_ADMIN","SYS_ADMIN","SYS_PTRACE","SYS_RESOURCE"]}}` | Security context for the manager container; requires SYS_PTRACE and SYS_ADMIN for CRIU, NET_ADMIN for eBPF qdiscs/filters, SYS_RESOURCE for memlock rlimit |
| manager.serviceMonitor | object | `{"additionalLabels":{},"enabled":false,"interval":"30s","metricRelabelings":[],"relabelings":[],"scrapeTimeout":"10s"}` | Create a headless Service and ServiceMonitor for Prometheus scraping |
| manager.serviceMonitor.additionalLabels | object | `{}` | Extra labels added to the ServiceMonitor — useful for matching a specific Prometheus instance via serviceMonitorSelector |
| manager.serviceMonitor.enabled | bool | `false` | Enable the ServiceMonitor and headless Service |
| manager.serviceMonitor.interval | string | `"30s"` | Prometheus scrape interval |
| manager.serviceMonitor.metricRelabelings | list | `[]` | Prometheus metric relabeling rules applied to scraped metrics after they are received |
| manager.serviceMonitor.relabelings | list | `[]` | Prometheus relabeling rules applied to scraped metrics |
| manager.serviceMonitor.scrapeTimeout | string | `"10s"` | Prometheus scrape timeout |
| manager.statusEvents | bool | `true` | Create Kubernetes events on scale-down and restore |
| manager.statusLabels | bool | `true` | Set status.zeropod.ctrox.dev/<container>: RUNNING|SCALED_DOWN labels on pods |
| manager.trackerIgnoreLocalhost | bool | `false` | Ignore localhost connections in the socket tracker; useful when probes or sidecars connect on loopback |
| nodeSelector | object | `{"zeropod.ctrox.dev/node":"true"}` | Node selector for the DaemonSet; nodes must be labeled zeropod.ctrox.dev/node=true to receive the DaemonSet |
| rke2 | bool | `false` | Enable RKE2-specific host paths and pass -runtime=rke2 to the installer |
| runtimeClass.name | string | `"zeropod"` | Name of the RuntimeClass; reference in pod specs with runtimeClassName: zeropod |
| tolerations | list | `[{"operator":"Exists"}]` | Tolerations for the DaemonSet; tolerates all taints by default to run on every labeled node regardless of taints |

## Distribution support

By default the chart targets a standard containerd installation. Set the flag that matches your
environment if you are running GKE, K3s or RKE2. Each flag rewrites the containerd host paths
and passes the appropriate flags to the installer init container. Only one flag should be set at
a time.

| Flag | containerd config path | containerd run path | zeropod opt path |
|------|----------------------|--------------------|--------------------|
| *(default)* | `/etc/containerd` | `/run/containerd` | `/opt/zeropod` |
| `gke: true` | `/etc/containerd` | `/run/containerd` | `/var/lib/toolbox/zeropod` |
| `k3s: true` | `/var/lib/rancher/k3s/agent/etc/containerd` | `/run/k3s/containerd` | `/var/lib/rancher/k3s/agent/containerd` |
| `rke2: true` | `/var/lib/rancher/rke2/agent/etc/containerd` | `/run/k3s/containerd` | `/var/lib/rancher/rke2/agent/containerd` |

## Uninstall

To remove zeropod from all labeled nodes, set `installer.uninstall=true` and re-apply the chart.
The installer init container will remove zeropod binaries and restore the original containerd
configuration on each node, then restart the runtime. After uninstall is finished, helm chart installation can be removed.
