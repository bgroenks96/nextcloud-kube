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

The size of the data partition can be set as an argument to `deploy`:

`./deploy -s 100Gi <RELEASE> <DOMAIN>`

Other options can be supplied; `dry-run` will test the configuration without applying any changes, and `staging` will use the Let's Encrypt staging certificate endpoint to avoid exceeding the quotas for your domain:

`./deploy --dry-run --staging <RELEASE> <DOMAIN>`

After deployment, generated configuration files for that release will be placed in the `releases` directory. You can change the configuration options and upgrade the existing deployment using `./update <RELEASE>` where `<RELEASE>` is the same identifier you used when creating the deployment.

Note that the persistent storage configuration options in `storage.yaml` and `nextcloud.values.yaml` currently assume storage type of `do-block-storage` which is specific to DigitalOcean Kubernetes deployments. If you are using another cloud provider, **you will need to change these values**. Check your provider's documentation on persistent storage.

These scripts and configuration files are provided WITHOUT WARRANTY and by using it you agree to release the author of any potential damages resulting from accidental loss or corruption of your data.
