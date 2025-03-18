# (Unofficial) Documenso Helm Chart

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/documenso)](https://artifacthub.io/packages/search?repo=documenso)

This repository provides a Helm chart to deploy [Documenso](https://github.com/Documenso/documenso), a modern open-source document signing platform.

## Prerequisites

- Kubernetes 1.20+
- Helm 3.x
- Persistent storage provisioner (for document storage)
- (Optional) Ingress Controller (e.g., Nginx)

## Installation

To install the chart using Helm:

```sh
helm repo add rizkyfaza20 https://rizkyfaza20.github.io/helm-charts
helm repo update
helm install my-documenso rizkyfaza20/documenso-chart
```

## Configuration

You can customize the deployment by modifying the `values.yaml` file. To override values, use:

```sh
helm install my-documenso rizkyfaza20/documenso-chart --values=my-values.yaml
```

Or override a specific setting:

```sh
helm install my-documenso rizkyfaza20/documenso-chart --set service.type=LoadBalancer
``` 

## Upgrading

To upgrade an existing release:

```sh
helm upgrade my-documenso rizkyfaza20/documenso-chart
```

## Uninstallation

To remove the Helm release:

```sh
helm uninstall my-documenso
```

## Contributing

Feel free to contribute by opening issues or submitting pull requests.

1. Fork the repository
2. Create a feature branch
3. Make your changes and test them
4. Submit a pull request