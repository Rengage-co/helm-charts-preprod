# Rengage CE Deployment Guide

---

## Overview

**Rengage Community Edition (CE)** is a self-hosted version of Rengage that can be deployed within a customer's infrastructure. This enables customers to manage Rengage securely in their own environment instead of relying on the cloud-hosted solution.

Rengage CE is built on a scalable microservices architecture deployed and managed via Kubernetes. It provides core functionality such as:
- Audience management  
- Content editing  
- Journey orchestration  
- Delivery channel integration  
- Agentic workflows

---

## Architecture & Requirements

### Components
- `rengage-ce-depandency`: Core dependencies required to run Rengage (e.g., Redis)  
- `rengage-ce`: Main Rengage services and APIs  
- `prometheus`: Monitoring infrastructure

### Prerequisites
- Kubernetes cluster (v1.19 or higher)  
- Helm (v3.7 or higher)  
- `kubectl` access to your Kubernetes cluster

### Minimum System Requirements

To deploy Rengage CE with minimal load:

<<<<<<< HEAD
To install the rengage-ce-dependency chart:

    helm install my-rengage-ce-dep rengage-preprod/rengage-ce-dependency
=======
**Option 1: Single-node cluster**
- 16 CPU cores  
- 64 GB memory

**Option 2: Multi-node cluster**
- At least 2 Kubernetes worker nodes  
- Each with:  
  - 8 CPU cores  
  - 32 GB memory
>>>>>>> 9fd657ad165494356cd314fda87a01de4f3df3b7

> For production or high-traffic environments, allocate additional resources based on expected load.

---

## Quick Start

### 1. Install Helm
If Helm is not already installed, refer to the official [Helm documentation](https://helm.sh/docs/) for setup instructions.

> Helm charts are package bundles that simplify deploying Kubernetes applications with templated resources and configurable values.

### 2. Add Rengage Helm Repository
```bash
helm repo add rengage-preprod https://Rengage-co.github.io/helm-charts-preprod
helm repo update
```

> If you've already added the repo before, just run `helm repo update` to ensure you have the latest chart versions.

### 3. Load Firebase Credentials
Before installation, you must apply the Firebase `service_account` credentials via a secret file.  
Contact support to get the required `secret.yaml`.

```bash
kubectl apply -f secret.yaml
```

### 4. Install Dependencies
Install Rengage CE dependencies (e.g., Redis):

```bash
helm install my-rengage-ce-dep rengage-preprod/rengage-ce-depandency
```

### 5. Install the Main Application
Deploy the Rengage CE services:

```bash
helm install my-rengage-ce rengage-preprod/rengage-ce
```

### 6. Customize Configuration (Optional)

Download the default values file:

```bash
helm show values rengage-preprod/rengage-ce > my-values.yaml
```

Make your changes in `my-values.yaml` and install using:

```bash
helm install my-rengage-ce rengage-preprod/rengage-ce -f my-values.yaml
```

To upgrade the deployment after editing:

```bash
helm upgrade my-rengage-ce rengage-preprod/rengage-ce -f my-values.yaml
```

---

## Uninstall Rengage CE

To completely remove your deployment:

```bash
helm uninstall my-rengage-ce
helm uninstall my-rengage-ce-dep
```

---

## Configuration Options

You can customize most aspects of your deployment via `values.yaml` or `--set` flags.

### Microservice Configuration

Each Rengage microservice (e.g., `nginx`, `aiagent`, `comment`, `frontend`, `template`, etc.) supports the following keys:

- `replicaCount`  
- `image.repository`, `image.tag`, `image.pullPolicy`  
- `env`: Environment variables  
- `imagePullSecrets`: For private image registries  
- `podAnnotations`, `podLabels`: Metadata  
- `podSecurityContext`, `securityContext`  
- `resources`: CPU and memory requests/limits  
- `livenessProbe`, `readinessProbe`: Health checks  
- `autoscaling`: Enable HPA with min/max replicas  
- `volumes`, `volumeMounts`

### Service Configuration
- `service.type`: `ClusterIP`, `NodePort`, or `LoadBalancer`  
- `service.port`: Custom port settings

### Ingress Configuration
- `ingress.enabled`  
- `ingress.className`  
- `ingress.annotations`  
- `ingress.hosts`: Define routes and paths  
- `ingress.tls`: For HTTPS termination

---

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

---

## Dependency Chart Configuration

The `rengage-ce-depandency` chart allows customization of Redis and other system-level dependencies.

---

## View All Available Options

To inspect all configurable parameters:

```bash
helm show values rengage-preprod/rengage-ce > my-values.yaml
```

Edit and install/upgrade using the customized file.
