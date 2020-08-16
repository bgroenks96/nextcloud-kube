### NextCloud via Kubernetes

These scripts provide a straight-forward, preconfigured NextCloud deployment on a Kubernetes cluster.

Prerequisities:

- `kubectl` and `helm` are both accessible on the system PATH.

- Default stable Helm repository has been added and updated. This can be done via:

`helm repo add stable https://kubernetes-charts.storage.googleapis.com/`

First initialize necessary cluster services by running

`./init-cluster <EMAIL>`

replacing `<EMAIL>` with your email address (this is for TLS certificate registration). This script will install `nginx` and configure the certificate issuer for the cluster. It only needs to be run **once per cluster**.

To create a new NextCloud deployment, run:

`./deploy <RELEASE> <DOMAIN>`

replacing `<RELEASE>` with the name of your release/deployment (e.g. "nextcloud") and `<DOMAIN>` with the domain name you will use to access it (e.g. nextcloud.example.com).

The script will output an IP address which must be the target of a DNS A-record for your domain.

*Warning: The data volume is created automatically during installation. Deleting this deployment via `helm delete` will delete your data, so make sure to create a back-up first.*

The size of the data partition, as well as other various options, can be configured **before deployment** via `templates/nextcloud.values.yaml`.

After deployment, generated configuration files for that release will be placed in the `releases` directory. You can change the configuration options and upgrade the existing deployment using `upgrade <RELEASE>` where `<RELEASE>` is the same identifier you used when creating the deployment.

Please note that these scripts and configuration files are provided WITHOUT WARRANTY and by using it you agree to release the author of any potential damages resulting from accidental loss or corruption of your data.
