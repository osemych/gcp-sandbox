# Prereqiusites

## Google Cloud Platform

This document uses the [Google Cloud Platfrom](http://cloud.google.com) resources to provision Kubernetis cluster with Jenkins master node and on-demand workers.

## Google Cloud Platform SDK

Enable the [Google Compute Engine and Google Container Engine APIs](https://console.cloud.google.com/flows/enableapi?apiid=compute_component,container)

### Install the Google Cloud SDK
Follow the Google Cloud SDK [documentation](https://cloud.google.com/sdk/) to install and configure the `gcloud` command line utility.

### Install the Kubernetis SDK
From your shell or terminal window, install the `kubectl` command-line tool
```
gcloud components install kubectl
```

### Set a Default Compute Region and Zone
Set default compute region:
```
gcloud config set compute/region eu-west2
```

Set default compute zone:
```
gcloud config set compute/zone eu-west2-a
```

> Use the `gcloud compute zones list` command to view available regions and zones.

## Prepare persistent storage disk resource
To create disk resource run:
```
gcloud compute disks create --size 10GB jenkins-disk
```

Attach created disk to running instance:
```
gcloud compute instances attach-disk [INSTANCE_NAME] --disk jenkins-disk
```

> Use any available instance -- it can be a temporary instance to create disk partition and filesystem.

Follow this [giude](https://cloud.google.com/compute/docs/disks/add-persistent-disk#formatting) to format and prepare the disk for use.

# Prepare environment

Clone sandbox repository in your shell, terminal window or cloud shell, then `cd` into that dir:
git clone https://github.com/osemych/gcp-sandbox.git

## Create Kubernetis cluster

Kubernetis cluster will be managed by Google Container Engine. Provision the cluster with `gcloud`:
```
gcloud container clusters create jenkins-cd \
  --machine-type n1-standard-2 \
  --num-nodes 3 \
  --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
```

> Use `gcloud compute machine-types list` command to list available instances types

Once cluster creation completes download the credentials for your cluster using `gcloud` (optional):
```
gcloud container cluster get-credentials jenkins-cd
```

