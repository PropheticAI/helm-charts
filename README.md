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
| `dataSources.packet` | Enable packet capture | `true` |
| `dataSources.netflow` | Enable NetFlow/IPFIX | `false` |
| `dataSources.suricata` | Enable Suricata logs | `false` |
| `packetCapture.dpi` | Deep packet inspection | `true` |
| `packetCapture.interfaces` | NICs to monitor (empty = all) | `[]` |
| `packetCapture.excludeInterfaces` | NICs to exclude | `[]` |
| `performance.slim` | Slim mode for constrained nodes | `false` |
| `persistence.enabled` | Persist state across restarts | `true` |
| `persistence.hostPath` | Host path for state storage | `/var/lib/prophet-collector` |
| `tolerations` | Pod tolerations | `[{operator: Exists}]` |
| `tags` | Tags to add to all flows | `[]` |

See [values.yaml](charts/prophet-collector/values.yaml) for all options.
