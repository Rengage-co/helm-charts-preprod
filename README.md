## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

---

### 📌 Minimum System Requirements

To successfully run the Rengage CE charts in a Kubernetes cluster, the following minimum system resources are required:

- **Option 1: Single node**
  - 16 CPU cores
  - 64 GB memory

- **Option 2: Multi-node cluster**
  - At least 2 Kubernetes worker nodes
  - Each with:
    - 8 CPU cores
    - 32 GB memory

These requirements ensure that all services can be scheduled and operate reliably under lightweight workloads. For production deployments with higher traffic or larger datasets, consider provisioning additional resources.

---

Once Helm has been set up correctly, add the repo as follows:

    helm repo add rengage-preprod https://Rengage-co.github.io/helm-charts-preprod

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
rengage` to see the charts.

Before running the helm install, you need to execute the following command to load the Firebase service_account information. To create the required `secret.yaml` file, please contact our support team. 

    kubectl apply -f secret.yaml

To install the rengage-ce-depandency chart:

    helm install my-rengage-ce-dep rengage-preprod/rengage-ce-depandency

To install the rengage-ce chart:

    helm install my-rengage-ce rengage-preprod/rengage-ce

To download the rengage-ce chart values:

    helm show values rengage-preprod/rengage-ce > my-values.yaml

To install the rengage-ce chart with customized values:

    helm install my-rengage-ce rengage-preprod/rengage-ce -f my-values.yaml
    
To upgrade the rengage-ce chart:

    helm upgrade my-rengage-ce rengage-preprod/rengage-ce -f my-values.yaml

To uninstall the chart:

    helm uninstall my-rengage-ce
    helm uninstall my-rengage-ce-dep