# Heat template examples

This repository is a collection of heat templates for certain use cases. Copy the <example>-env.dist.yaml
to <example>-env.yaml and add your values. Then create the stack with

```shell
openstack create stack -e <example>-env.yaml -t <example>.yaml <example-stack> 
```

## docker.yaml

This template shows how to create a private network for a server with a loadbalancer that
gets assigned an existing floating ip address.

The server is created with an image that already has Docker installed. 

The server uses cloud-config to run an image.

The loadbalancer has a rule to redirect http requests for a host to the container.

### Prerequisites

1. Create a key pair.
2. Create a floating ip.
3. Create a DNS record for the floating ip.

## talos-cluster/talos-cluster.yaml

This template sets up a Kubernetes cluster based on Talos Linux.
It includes
- network infrastructure with public and private subnet
- load-balancer and listeners for endpoints of Talos Linux and Kubernetes
- one node as Kubernetes control plane
- one node as permanent single worker
- an autoscaling group of nodes for additional workers

### Prerequisites

You need an OpenStack project with
- a public network
- a dns zone
- image with Talos Linux

### Setup

1. Create `talos-cluster-env.yaml` (see `talos-cluster-env.example.yaml`) and adapt it to your needs
1. Create Talos config
1. Create stack by applying heat template
1. Bootstrap Talos

```shell
cd talos-cluster
vim talos-cluster-env.yaml
talosctl gen config talos-cluster https://talos-cluster.example.com --kubernetes-version 1.29.4 --additional-sans talos-cluster.example.com
openstack stack create -e talos-cluster-env.yaml -t cluster.yaml talos-cluster
talosctl bootstrap --nodes control-plane --endpoints talos-cluster.example.com --talosconfig=./talosconfig
```