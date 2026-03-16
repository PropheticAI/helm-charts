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
  --set provisionToken=<your-provision-token>
```

Get your provision token from the Prophet UI: **Nodes > New Node** or **Nodes > Profiles > Deploy**.

Service configuration (packet capture, netflow, suricata, interfaces, performance) is managed by the controller via **node profiles** — not helm values.

#### Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `provisionToken` | Provision token for node registration (required) | `""` |
| `profileId` | Profile ID to inherit config from (optional) | `""` |
| `env` | Prophet environment (`prod` or `dev`) | `prod` |
| `image.repository` | Collector image | `propheticai/go-collector` |
| `image.tag` | Image tag | `latest` |
| `imagePullSecrets` | Image pull secrets (for private repos) | `[]` |
| `persistence.hostPath` | Host path for state storage | `/var/lib/prophet-collector` |
| `resources` | Resource requests/limits | `{}` |
| `tolerations` | Pod tolerations | `[{operator: Exists}]` |
| `nodeSelector` | Node selector for scheduling | `{}` |
| `extraEnv` | Additional environment variables | `[]` |

See [values.yaml](charts/prophet-collector/values.yaml) for all options.

#### Examples

Use a private debug image:
```bash
helm install prophet-collector prophet/prophet-collector \
  --set provisionToken=<token> \
  --set image.repository=propheticai/go-collector-debug \
  --set image.tag=dev \
  --set imagePullSecrets[0].name=dockerhub-credentials
```

Connect to dev environment:
```bash
helm install prophet-collector prophet/prophet-collector \
  --set provisionToken=<token> \
  --set env=dev
```
