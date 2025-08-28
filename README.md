# PeaNUT Helm Chart

[![Helm](https://img.shields.io/badge/Helm-v3-blue.svg)](https://helm.sh)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

A Helm chart for deploying [PeaNUT](https://github.com/Brandawg93/PeaNUT) - a tiny dashboard for Network UPS Tools (NUT) on Kubernetes.

## Description

PeaNUT is a web-based dashboard that provides a user-friendly interface for monitoring UPS devices through Network UPS Tools (NUT). This Helm chart simplifies the deployment of PeaNUT on Kubernetes clusters.

## Features

- 🔋 Monitor multiple UPS devices
- 📊 Real-time status and metrics
- 🌐 Web-based dashboard
- 🔧 Configurable settings through environment variables
- 💾 Persistent configuration storage
- 🚀 Easy Kubernetes deployment with Helm
- 📈 Prometheus metrics support
- 🔗 API access for integration

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- A running NUT server with UPS devices configured

## Installation

### Add the Helm Repository

```bash
helm repo add peanut https://coreyj87.github.io/peanut-helm-chart
helm repo update
```

### Install the Chart

```bash
helm install peanut peanut/peanut
```

### Install with Custom Values

```bash
helm install peanut peanut/peanut -f values.yaml
```

## Configuration

The following table lists the configurable parameters and their default values:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | PeaNUT image repository | `brandawg93/peanut` |
| `image.tag` | Image tag | `latest` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `8080` |
| `ingress.enabled` | Enable ingress | `false` |
| `ingress.hosts[0].host` | Ingress hostname | `peanut.synik4l.net` |
| `peanut.env.WEB_HOST` | Web server hostname | `0.0.0.0` |
| `peanut.env.WEB_PORT` | Web server port | `8080` |
| `peanut.persistence.enabled` | Enable persistent storage | `true` |
| `peanut.persistence.size` | Storage size | `1Gi` |
| `peanut.persistence.storageClass` | Storage class | `""` |
| `resources.limits.cpu` | CPU limit | `500m` |
| `resources.limits.memory` | Memory limit | `512Mi` |
| `resources.requests.cpu` | CPU request | `100m` |
| `resources.requests.memory` | Memory request | `128Mi` |

## Environment Variables

PeaNUT supports several environment variables for configuration:

| Variable | Description | Default |
|----------|-------------|---------|
| `WEB_HOST` | Hostname of web server | `localhost` |
| `WEB_PORT` | Port of web server | `8080` |
| `WEB_USERNAME` | Username for authentication | `undefined` |
| `WEB_PASSWORD` | Password for authentication | `undefined` |
| `BASE_PATH` | Base path for reverse proxy | `undefined` |
| `DISABLE_CONFIG_FILE` | Disable config file saving | `undefined` |

## Example Values File

```yaml
# Enable ingress
ingress:
  enabled: true
  annotations:
    external-dns.alpha.kubernetes.io/hostname: peanut.example.com
    external-dns.alpha.kubernetes.io/target: "192.168.1.200"
  hosts:
    - host: peanut.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific

# Configure PeaNUT
peanut:
  env:
    WEB_USERNAME: "admin"
    WEB_PASSWORD: "secure-password"
  
  persistence:
    enabled: true
    storageClass: "longhorn"
    size: 2Gi

# Resource limits
resources:
  limits:
    cpu: 1000m
    memory: 1Gi
  requests:
    cpu: 200m
    memory: 256Mi
```

## Usage

### Accessing PeaNUT

Once deployed, you can access PeaNUT through:

1. **Port Forward** (for testing):
   ```bash
   kubectl port-forward svc/peanut 8080:8080
   ```
   Then open http://localhost:8080

2. **Ingress** (if enabled):
   Access via the configured hostname

3. **Service** (within cluster):
   Use the service name `peanut` on port `8080`

### Configuring NUT Connection

PeaNUT requires connection to a NUT server. Configure this through the web interface or by editing the configuration files in the persistent volume.

## Monitoring

### Prometheus Metrics

PeaNUT exposes Prometheus metrics at `/api/v1/metrics`. To scrape these metrics, add the following annotations to your service:

```yaml
service:
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/path: "/api/v1/metrics"
    prometheus.io/port: "8080"
```

### API Endpoints

PeaNUT provides a REST API for integration:

- `GET /api/v1/devices` - List all UPS devices
- `GET /api/v1/devices/{ups}` - Get specific UPS information
- `GET /api/v1/metrics` - Prometheus metrics

See the [PeaNUT API documentation](https://github.com/Brandawg93/PeaNUT#api) for complete details.

## Upgrading

To upgrade the chart:

```bash
helm repo update
helm upgrade peanut peanut/peanut
```

## Uninstalling

To uninstall the chart:

```bash
helm uninstall peanut
```

**Note**: This will not delete the PersistentVolumeClaim. To delete it:

```bash
kubectl delete pvc peanut-config
```

## Troubleshooting

### Common Issues

1. **PeaNUT can't connect to NUT server**
   - Ensure NUT server is accessible from the Kubernetes cluster
   - Check network policies and firewall rules
   - Verify NUT server configuration allows connections

2. **Persistent volume issues**
   - Ensure the specified storage class exists
   - Check if the cluster has sufficient storage

3. **Authentication not working**
   - Verify `WEB_USERNAME` and `WEB_PASSWORD` are set correctly
   - Check if environment variables are properly mounted

### Logs

View PeaNUT logs:

```bash
kubectl logs deployment/peanut
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This Helm chart is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.

The PeaNUT application is licensed under the Apache License 2.0. See the [original repository](https://github.com/Brandawg93/PeaNUT) for details.

## Acknowledgments

- [PeaNUT](https://github.com/Brandawg93/PeaNUT) by [Brandawg93](https://github.com/Brandawg93)
- [Network UPS Tools (NUT)](https://networkupstools.org/)
