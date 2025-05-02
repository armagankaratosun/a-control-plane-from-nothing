# A Control Plane From Nothing

Welcome to the Control Plane From Nothing project — because sometimes, you just need to build a cloud from scratch and feel powerful doing it.

This repository provides an Ansible playbook for configuring and deploying a modern control plane environment. Perfect for platform engineers who like YAML with their caffeine and want hands-on experience with Cluster API (CAPI), Sveltos, and Crossplane.

## Requirements

You only need one machine to rule them all (for now):

* CPU: 8 vCPUs
* RAM: 16 GB
* Disk: 40 GB
* OS: Ubuntu 22.04 — others might work, but you’re on your own there

SSH with passwordless sudo is also needed. 

## Quickstart
Control the control plane. YAML responsibly. Hack the planet.
### Inventory and Configuration

This playbook requires a standard Ansible `hosts` or `inventory` file to target the control plane node.

Example:

```bash
[controlplane]
10.0.0.1 ansible_user=ubuntu
```

Note: Currently only single-node Kubernetes deployments are supported. Multi-node support is coming soon to a playbook near you!

### Configuration

Configuration for the Kubernetes and platform components is provided via `values.yaml`.

| Key                               | Type     | Description                                                                 |
|----------------------------------|----------|-----------------------------------------------------------------------------|
| `kubectl_version`                | string   | Version of `kubectl` to install                                            |
| `rke2.token`                     | string   | RKE2 token for node enrollment                                             |
| `rke2.cni`                       | string   | CNI plugin to use (e.g., `cilium`)                                         |
| `rke2.cluster_cidr`             | string   | CIDR for pod networking                                                    |
| `rke2.service_cidr`             | string   | CIDR for Kubernetes services                                               |
| `rke2.domain`                   | string   | Cluster DNS domain                                                         |
| `rke2.version`                  | string   | RKE2 version to install                                                    |
| `rke2.channel`                  | string   | RKE2 release channel                                                       |
| `addons.certmanager`            | boolean  | Enable Cert Manager                                                        |
| `addons.crossplane`             | boolean  | Enable Crossplane                                                          |
| `addons.cluster_api`            | boolean  | Enable Cluster API                                                         |
| `addons.external_secrets_operator` | boolean | Enable External Secrets Operator                                           |
| `addons.certmanager_version`    | string   | Version of Cert Manager                                                    |
| `addons.crossplane_version`     | string   | Version of Crossplane                                                      |
| `addons.cluster_api_version`    | string   | Version of Cluster API                                                     |
| `cluster_api.core.cluster_api.version` | string | Cluster API Core provider version                                          |
| `cluster_api.bootstrap.rke2.version`  | string | RKE2 bootstrap provider version                                            |
| `cluster_api.controlPlane.rke2.version`| string | RKE2 control plane provider version                                        |
| `providers.openstack`           | boolean  | Enable OpenStack provider                                                  |
| `providers.aws`                 | boolean  | Enable AWS provider                                                        |
| `providers.gcp`                 | boolean  | Enable GCP provider                                                        |
| `openstack.auth_url`            | string   | OpenStack authentication URL                                               |
| `openstack.application_credential_id` | string | OpenStack application credential ID                                       |
| `openstack.application_credential_secret` | string | OpenStack application credential secret                                   |
| `openstack.region`              | string   | OpenStack region                                                           |
| `openstack.interface`           | string   | OpenStack interface (e.g., `public`)                                       |

Example `values.yaml`:

```yaml
kubectl_version: v1.33.0
rke2:
  token: "token"
  cni: "cilium"
  cluster_cidr: "10.42.0.0/16"
  service_cidr: "10.43.0.0/16"
  domain: "control-plane.local"
  version: "v1.31.7+rke2r1"
  channel: "stable"
addons:
  certmanager: true
  crossplane: true
  cluster_api: true
  external_secrets_operator: false
  certmanager_version: v1.17.2
  crossplane_version: 1.10.0
  cluster_api_version: 0.19.0
cluster_api:
  core:
    cluster_api:
      version: "v1.9.3"
  bootstrap:
    rke2:
      version: "v0.14.0"
  controlPlane:
    rke2:
      version: "v0.14.0"
providers:
  openstack: true
  aws: false
  gcp: false
openstack:
  auth_url: ""
  application_credential_id: ""
  application_credential_secret: "--"
  region: ""
  interface: ""
```

### Getting Started

1. Clone this repository.

2. Create or edit your inventory file.

3. Create or customize your `values.yaml` file.

4. Run the playbook:

`ansible-playbook -i inventory installer.yaml -e @values.yaml`

## What It Does

* RKE2 Kubernetes cluster with chosen CNI
* Cluster API with selected providers
* Add-ons like Cert Manager, Crossplane, and optionally External Secrets Operator
* Sveltos for policy-based add-on management

## How to Contribute?

Open issues, fork it, PR it — you know the drill.

## Author

Armagan Karatosun

# License 

Apache License Version 2.0