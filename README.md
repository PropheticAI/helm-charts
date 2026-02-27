# Prophet Helm Charts

Helm charts for deploying Prophet components on Kubernetes.

## Usage

```bash
helm repo add prophet https://propheticai.github.io/helm-charts
helm repo update
```

## Available Charts

### prophet-collector

Deploys the Prophet network collector as a DaemonSet on every node in your cluster.

```bash
helm install prophet-collector prophet/prophet-collector \
  --set orgToken=<your-org-token>
```

Get your org token from the Prophet UI: **Nodes > New Node > Organization Token**

After installation, approve the pending devices in the Prophet UI under **Nodes > Pending Approvals**.

#### Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `orgToken` | Organization token for device authorization (required) | `""` |
| `env` | Prophet environment (`prod` or `dev`) | `prod` |
| `image.repository` | Collector image | `propheticai/go-collector` |
| `image.tag` | Image tag | `latest` |
| `imagePullSecrets` | Image pull secrets (for private repos) | `[]` |
| `dataSources.packet` | Enable packet capture | `true` |
| `dataSources.netflow` | Enable NetFlow/IPFIX | `false` |
| `dataSources.suricata` | Enable Suricata logs | `false` |
| `packetCapture.dpi` | Deep packet inspection | `true` |
| `packetCapture.afpacket` | Use AF_PACKET (recommended) | `true` |
| `packetCapture.interfaces` | NICs to monitor (empty = all) | `[]` |
| `packetCapture.excludeInterfaces` | NICs to exclude | `[]` |
| `performance.numWorkers` | Packet decoder workers (0 = auto) | `0` |
| `performance.numSpoolers` | Parallel spooler instances | `4` |
| `performance.slim` | Slim mode for constrained nodes | `false` |
| `persistence.enabled` | Persist state across restarts | `true` |
| `persistence.hostPath` | Host path for state storage | `/var/lib/prophet-collector` |
| `resources` | Resource requests/limits | `{}` |
| `tolerations` | Pod tolerations | `[{operator: Exists}]` |
| `nodeSelector` | Node selector for scheduling | `{}` |
| `tags` | Tags to add to all flows | `[]` |
| `extraEnv` | Additional environment variables | `[]` |

See [values.yaml](charts/prophet-collector/values.yaml) for all options.

#### Examples

Use a private debug image:
```bash
helm install prophet-collector prophet/prophet-collector \
  --set orgToken=<token> \
  --set image.repository=propheticai/go-collector-debug \
  --set image.tag=dev \
  --set imagePullSecrets[0].name=dockerhub-credentials
```

Monitor only specific interfaces:
```bash
helm install prophet-collector prophet/prophet-collector \
  --set orgToken=<token> \
  --set packetCapture.interfaces={eth0}
```

Connect to dev environment:
```bash
helm install prophet-collector prophet/prophet-collector \
  --set orgToken=<token> \
  --set env=dev
```
