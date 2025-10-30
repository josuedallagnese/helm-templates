# AKS Helm Chart Templates

A collection of reusable Helm chart templates for deploying different workload types.

## Available Templates

- **api-template**: Template for deploying REST APIs and web services

## Repository Structure

```
aks/
├── README.md
├── api-template/           # REST API and web service deployments
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
```

## API Template Features

- **Deployment**: Configurable container deployment with resource limits and probes
- **Service**: NodePort service with health check endpoints
- **Ingress**: Azure Application Gateway integration with SSL support
- **Autoscaling**: Horizontal Pod Autoscaler (HPA) with CPU and memory-based scaling
- **Health Checks**: Startup, readiness, and liveness probes


## Getting Started

### Using the API Template

1. **Navigate to the template**:
   ```bash
   cd api-template
   ```

2. **Customize values**:
   Edit `values.yaml` to configure your application:
   ```yaml
   name: my-api
   image:
     repository: your-registry/your-image
     tag: "1.0.0"
   ingress:
     hosts:
       - host: your-domain.com
   ```

3. **Install the chart**:
   ```bash
   helm install my-api . -f values.yaml
   ```

### Configuration

Key configuration options in `values.yaml`:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `name` | Application name | `my-api` |
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Container image repository | `nginx` |
| `image.tag` | Container image tag | `latest` |
| `service.port` | Service port | `8080` |
| `ingress.enabled` | Enable ingress | `true` |
| `ingress.ingressClassName` | Ingress class name | `azure-application-gateway` |
| `ingress.annotations` | Ingress annotations | See values.yaml |
| `autoscaling.enabled` | Enable HPA | `true` |
| `autoscaling.minReplicas` | Minimum replicas | `1` |
| `autoscaling.maxReplicas` | Maximum replicas | `3` |

## Validation and Testing

### Validate the chart

```bash
# Lint the chart for issues
helm lint ./api-template

# Render templates without installing
helm template my-api ./api-template

# Dry-run installation
helm install --dry-run --debug my-api ./api-template
```

### Generate output files for validation

```bash
# Generate all manifests to a file
helm template my-api ./api-template > output.yaml

# Generate to separate files in a directory
helm template my-api ./api-template --output-dir ./rendered-manifests

# Validate with kubectl
kubectl apply --dry-run=client -f output.yaml
```

## Upgrade

```bash
helm upgrade my-api ./api-template -f values.yaml
```

## Uninstall

```bash
helm uninstall my-api
```