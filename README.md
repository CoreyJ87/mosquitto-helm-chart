# Mosquitto MQTT Broker Helm Chart

This Helm chart deploys Eclipse Mosquitto MQTT broker on Kubernetes using the official [Eclipse Mosquitto Docker image](https://hub.docker.com/_/eclipse-mosquitto).

## Features

- Eclipse Mosquitto MQTT broker
- Persistent storage for data and configuration
- Support for MQTT, MQTT over TLS, and WebSockets
- Configurable authentication and logging
- Kubernetes-native deployment with StatefulSet

## Installation

```bash
# Install the chart
helm install mosquitto ./mosquitto-helm-chart

# Install with custom values
helm install mosquitto ./mosquitto-helm-chart -f custom-values.yaml

# Upgrade
helm upgrade mosquitto ./mosquitto-helm-chart
```

## Configuration

The following table lists the configurable parameters and their default values:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Mosquitto image repository | `eclipse-mosquitto` |
| `image.tag` | Mosquitto image tag | `2.0.18` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `replicaCount` | Number of replicas | `1` |
| `service.type` | Service type | `ClusterIP` |
| `service.mqtt.port` | MQTT port | `1883` |
| `service.mqtts.port` | MQTT over TLS port | `8883` |
| `service.websockets.port` | WebSocket port | `9001` |
| `persistence.enabled` | Enable persistent storage | `true` |
| `persistence.storageClass` | Storage class | `longhorn` |
| `persistence.size` | Storage size | `1Gi` |
| `resources.requests.memory` | Memory request | `64Mi` |
| `resources.requests.cpu` | CPU request | `50m` |
| `resources.limits.memory` | Memory limit | `128Mi` |
| `resources.limits.cpu` | CPU limit | `100m` |

## Usage

After installation, the MQTT broker will be available at:

- **MQTT**: `<release-name>-mosquitto:1883`
- **MQTT over TLS**: `<release-name>-mosquitto:8883`
- **WebSockets**: `<release-name>-mosquitto:9001`

### Setting up authentication

The default configuration disables anonymous access. To set up users:

1. Exec into the pod:
   ```bash
   kubectl exec -it <pod-name> -- sh
   ```

2. Create password file:
   ```bash
   mosquitto_passwd -c /mosquitto/config/passwords username
   ```

3. Restart the pod to apply changes.

## Uninstalling

```bash
helm uninstall mosquitto
```

## License

This chart is licensed under the Apache 2.0 License. 