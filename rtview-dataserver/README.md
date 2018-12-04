# Overview

RTView Dataserver description

# Installation

## Click to Deploy

Bootstrap your RTView Dataserver instance in a minutes. Use Google Cloud Platform Marketplace to deploy with a few clicks!

https://console.cloud.google.com/launcher/kubernetes/config/aerospike-prod/aerospike-server-enterprise

## Command line

RTView Dataserver has support for manual deployment using Docker and Kubernetes.

### Prerequisites

#### Software
1. Docker agent installed (https://www.docker.com/)
2. GNU Make
3. Google Cloud SDK (https://cloud.google.com/sdk/docs/quickstarts)
4. Kubectl tool (`gcloud components install kubectl`)

#### Kubernetes cluster

Firstly, You have to login to Google Cloud Platform

```
# Login to Google Cloud
gcloud auth login

# Setting default project and compute zone
gcloud config set project my-project-name
gcloud config set compute/zone us-west1-b
```

Please follow on-screen steps to procced.

Secondly, You have to create dedicated service account to communicate with Google Cloud Platform APIs.

```
gcloud iam service-accounts create <my-account-name>
gcloud iam service-accounts keys create key.json
  --iam-account <my-account-name>@<my-project>.iam.gserviceaccount.com
export GOOGLE_APPLICATION_CREDENTIALS="$(pwd)/key.json"
```

Lastly, You have to add docker credentials helper to support Google Cloud Registry

```
gcloud auth configure-docker
```

Create Kubernetes cluster

```
# Create the cluster
gcloud beta container clusters create cluster-1 \
  --machine-type "n1-standard-1" \
  --num-nodes 3

# Get credentials for kubectl utility
gcloud container clusters get-credentials cluster-1
```

#### Application Resource Defintion

```
make crd/install
```

Creation of Application Resource is required only once per cluster.

#### License secret

TODO

### Installation

Installation of RTView Dataserver have predefined defaults, but as a user you are capable of changing some of settings. Full list is available in section [Available variables](#available-variables). Below an example of deployment with one variable changed.

```
export NAME=rtview-dataserver-2

make app/install
```

Whole installation takes 2-3 minutes depending on machine type.
Make sure if everything is up and running by running following command.

```
kubetl get pods -l app.kubernetes.io/instance=rtview-dataserver-2
```

Every pod should be running and ready (`1/1`)

#### Available variables
Name | Description |  Default value
-----|-------------|--------------
`NAME` | Name of a helm deployment | `rtview-dataserver-1`
`NAMESPACE` | Namespace of kubernetes deployment | `default`
`REGISTRY` | Namespace of kubernetes deployment | `gcr.io/sl-rtview`
`TAG` | Tag of docker image. Available tags: `4.2.0.0`, `latest` | `latest`
`VARIANT` | Variant of Dataserver. Available variants: `""`, `kafka`, `solace` | `""`
`RTV_USERNAME` | Username | `"demo"`
`RTV_PASSWORD` | Password | `"demo"`

# First steps
## Expose your RTView Dataserver instance

```
kubectl patch service/rtview-dataserver-2-rtview-security-proxy --namespace default --patch '{"spec": {"type": "LoadBalancer"}}'
```

You have to wait a while for LoadBalancer to reserve public IP address. Once it will be ready Dataserver is going to be accessible. Get the URL using these commands.

```
export GATEWAY_IP=$(kubectl get service --namespace default rtview-dataserver-2-rtview-security-proxy -o jsonpath="{.status.loadBalancer.ingress[0].ip}")
echo "http://$GATEWAY_IP/"
```

# Backup and restore

TODO

# Deletion

```
make app/uninstall
```
