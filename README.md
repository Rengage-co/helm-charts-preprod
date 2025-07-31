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
- `rengage-ce-dependency`: Core dependencies required to run Rengage (e.g., Redis)  
- `rengage-ce`: Main Rengage services and APIs  
  - For a full list of services and deployment details, see  [**Rengage Microservice List**](https://github.com/Rengage-co/helm-charts-preprod/blob/main/rengage_microservice_list.md)

### Prerequisites
- Kubernetes cluster (v1.19 or higher)
  - [Setting up Kubernetes Cluster on GCP](https://cloud.google.com/kubernetes-engine/docs/quickstarts/create-cluster)
  - [Setting up Kubernetes Cluster on AWS](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html)
  - [Setting up Kubernetes cluster on Azure](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli)  
- Helm (v3.7 or higher)  
- <span style="color:red">kubectl</span> access to your Kubernetes cluster

### Official Guides for Granting `kubectl` Access and Permissions on AWS, GCP, and Azure

This section lists official documentation links and key points on how to configure and authorize `kubectl` access to Kubernetes clusters on AWS EKS, GCP GKE, and Azure AKS.

---

#### AWS EKS (Amazon Elastic Kubernetes Service)

- **Official Docs:**  
  - [Managing users or IAM roles for your cluster](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html)  
  - [Configure kubectl for Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html)

- **Key Points:**  
  - Use **IAM users or roles** bound to Kubernetes RBAC permissions to control access.  
  - Generate kubeconfig with:  
    ```bash
    aws eks --region <region> update-kubeconfig --name <cluster_name>
    ```  
  - Authentication uses **AWS IAM Authenticator** to validate identities.  
  - You can map IAM entities to Kubernetes users or groups in the `aws-auth` ConfigMap.

---

#### GCP GKE (Google Kubernetes Engine)

- **Official Docs:**  
  - [Granting RBAC permissions](https://cloud.google.com/kubernetes-engine/docs/how-to/role-based-access-control)  
  - [Authenticate to the cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl)

- **Key Points:**  
  - Access control is managed by Google Cloud **IAM roles** combined with Kubernetes **RBAC**.  
  - Generate kubeconfig using:  
    ```bash
    gcloud container clusters get-credentials <cluster_name> --zone <zone> --project <project_id>
    ```  
  - Users authenticate via their Google Cloud accounts and permissions are enforced both in GCP IAM and Kubernetes RBAC.  

---

#### Azure AKS (Azure Kubernetes Service)

- **Official Docs:**  
  - [Manage cluster access with RBAC and Azure Active Directory](https://learn.microsoft.com/en-us/azure/aks/manage-azure-rbac)  
  - [Connect to the cluster using kubectl](https://learn.microsoft.com/en-us/azure/aks/kubernetes-walkthrough#connect-to-the-cluster)

- **Key Points:**  
  - Supports **Azure Active Directory (AD)** integration combined with Kubernetes RBAC for fine-grained access control.  
  - Obtain kubeconfig with:  
    ```bash
    az aks get-credentials --resource-group <resource_group> --name <cluster_name>
    ```  
  - Users authenticate using Azure AD identities, and access is controlled via role assignments in Azure and Kubernetes role bindings.

---

#### Summary

| Cloud Provider | Authentication Mechanism           | Key Command to Configure `kubectl`                                  |
|----------------|----------------------------------|--------------------------------------------------------------------|
| AWS EKS        | IAM + Kubernetes RBAC            | `aws eks update-kubeconfig --region <region> --name <cluster_name>` |
| GCP GKE        | Google Cloud IAM + Kubernetes RBAC | `gcloud container clusters get-credentials <cluster_name> ...`       |
| Azure AKS      | Azure AD + Kubernetes RBAC       | `az aks get-credentials --resource-group <rg> --name <cluster_name>` |

---

> **Note:** Replace placeholders like `<region>`, `<cluster_name>`, `<resource_group>`, `<project_id>`, and `<zone>` with your actual values.


### Minimum System Requirements

To deploy Rengage CE with minimal load:

To install the rengage-ce-dependency chart:
    helm install my-rengage-ce-dep rengage-preprod/rengage-ce-dependency

**Option 1: Single-node cluster**
- 16 CPU cores  
- 64 GB memory

**Option 2: Multi-node cluster**
- At least 2 Kubernetes worker nodes  
- Each with:  
  - 8 CPU cores  
  - 32 GB memory

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

### 3. Enable Authentication

Rengage supports authentication through [Google Firebase](https://console.firebase.google.com/). Below is a high-level guide to setting up Firebase Authentication for your Rengage application.

#### Step 1: Create a Firebase Project
- Go to the [Firebase Console](https://console.firebase.google.com/) and sign in with your Google account.
- Create a new project or select an existing one.

#### Step 2: Enable Authentication Providers
- In the Firebase Console, navigate to **Build > Authentication**.
- Click on the **Sign-in method** tab.
- Enable the authentication providers you want to use (e.g., Email/Password, Google, Facebook, Phone).
- Follow the provider-specific setup instructions (such as setting up OAuth credentials for social logins).

#### Step 3: Add Firebase to Your Rengage Application
- After deploying the Rengage application in your cloud environment, open the application in a browser.
- The **initialization wizard** will launch automatically the first time you open the application.
- The wizard will guide you through integrating Firebase, which typically includes:
  - Configuring your application’s DNS domain.
  - Generating and uploading a private key file for your Firebase service account (required if you plan to use Rengage to send push notifications).


### 4. Install Dependencies
Install Rengage CE dependencies (e.g., Redis):

```bash
helm install my-rengage-ce-dep rengage-preprod/rengage-ce-dependency
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

### 7. DNS and SSL Configuration

After customizing your deployment, you may want to expose Rengage CE with a custom domain and enable HTTPS.

#### DNS Setup
- Point your domains to the external address of your cluster (Ingress / LoadBalancer).
  - frontend.ingress.hosts[0].host (e.g., `demo.rengage.net`)
  - feedmanager.ingress.hosts[0].host (e.g., `feedmanager.rengage.net`)
- Update your DNS records with your DNS provider (A record or CNAME).

#### SSL/TLS Setup
- You can enable HTTPS by configuring TLS:
  - **Use an existing certificate**: Create a Kubernetes TLS secret and reference it in `values.yaml`.
  - **Use cert-manager**: Automate the issuance of Let’s Encrypt certificates.
  - **Use DNS provider's proxy**: Some DNS or CDN providers (such as Cloudflare) can proxy traffic and handle SSL termination on their network edge.

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

The `rengage-ce-dependency` chart allows customization of Redis and other system-level dependencies.

---

## View All Available Options

To inspect all configurable parameters:

```bash
helm show values rengage-preprod/rengage-ce > my-values.yaml
```

Edit and install/upgrade using the customized file.
