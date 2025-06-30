# Rengage Kubernetes Deployment Guide

## Overview

The Rengage product is deployed to Kubernetes using Helm charts. This guide explains how to deploy Rengage and describes the configuration options customers can modify.

## Architecture Components

- **rengage-ce-depandency**: Core dependencies (e.g., Redis)
- **rengage-ce**: Main Rengage application and microservices
- **prometheus**: Monitoring stack

## Prerequisites

- Kubernetes cluster (v1.19+)
- Helm 3.7+
- kubectl access to your cluster

## Quick Start

### 1. Add the Helm Repository

```bash
helm repo add rengage-preprod https://Rengage-co.github.io/helm-charts-preprod
helm repo update
```

### 2. Install Dependencies

```bash
helm install my-rengage-ce-dep rengage-preprod/rengage-ce-depandency
```

### 3. Install the Main Application
```bash
helm install my-rengage-ce rengage-preprod/rengage-ce
```

## Uninstall
```bash
helm uninstall my-rengage-ce
helm uninstall my-rengage-ce-dep
```

# Configuration Options

Customers can customize their deployment by providing their own values file or using `--set` flags with Helm. Below are the main configuration areas for `rengage-ce`:

## Microservice Configuration

Each microservice (e.g., `nginx`, `aiagent`, `cache`, `comment`, `conductor`, `consumer`, `conductorconsumer`, `conductorui`, `dataretention`, `feedmanager`, `frontend`, `ingest`, `ingestwebhook`, `presentationlayer`, `template`, `userconsumer`) has its own section with these options:

- `replicaCount`: Number of pod replicas.
- `image.repository`, `image.tag`, `image.pullPolicy`: Container image settings.
- `env`: Environment variables.
- `imagePullSecrets`: For private registries.
- `podAnnotations`, `podLabels`: Pod-level metadata.
- `podSecurityContext`, `securityContext`: Security context for pods/containers.
- `resources`: CPU/memory requests and limits.
- `livenessProbe`, `readinessProbe`: Health checks.
- `autoscaling`: Enable HPA, set min/max replicas, CPU/memory targets.
- `volumes`, `volumeMounts`: Additional volumes and mounts.

## Service Configuration

- `service.type`: `ClusterIP`, `NodePort`, or `LoadBalancer`.
- `service.port`: Service port.

## Ingress Configuration

- `ingress.enabled`: Enable/disable ingress.
- `ingress.className`: Specify ingress class.
- `ingress.annotations`: Add custom annotations.
- `ingress.hosts`: Host and path rules.
- `ingress.tls`: TLS configuration.

## Example: Customizing a Microservice
```yaml
nginx:
  replicaCount: 2
  image:
    repository: myrepo/nginx
    tag: "1.21"
  env:
    - name: ENV1
      value: "value1"
  service:
    type: LoadBalancer
    port: 8080
  ingress:
    enabled: true
    hosts:
      - host: my-nginx.example.com
        paths:
          - path: /
            pathType: Prefix
    tls:
      - secretName: my-nginx-tls
        hosts:
          - my-nginx.example.com
```

## Dependency Chart

`rengage-ce-depandency` allows configuration of Redis and other dependencies.

## How to View All Configurable Options

To see all available options for a chart:

- Edit `my-values.yaml` as needed and install/upgrade with Helm.

For more details, see the full `values.yaml` in each chart directory.