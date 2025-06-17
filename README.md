## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

    helm repo add rengage-preprod https://Rengage-co.github.io/helm-charts-preprod

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
rengage` to see the charts.

To install the rengage-ce-depandency chart:

    helm install my-rengage-ce-dep rengage-preprod/rengage-ce-depandency

To install the rengage-ce chart:

    helm install my-rengage-ce rengage-preprod/rengage-ce

To uninstall the chart:

    helm uninstall my-rengage-ce
    helm uninstall my-rengage-ce-dep