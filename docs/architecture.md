# Architecture

## Overview

The Self-Hosted DevOps Platform is a production-style platform built on a single bare-metal server using **Proxmox VE** as the virtualization layer.

The platform is designed to demonstrate a complete delivery and runtime workflow, including:

- infrastructure provisioning
- host configuration management
- Kubernetes cluster bootstrap
- GitOps-based deployment
- secrets management
- observability
- stateful application dependencies
- CI integration with external systems

## Architecture Goals

The platform is designed to demonstrate the following capabilities:

- Infrastructure as Code with Terraform
- Configuration management with Ansible
- Kubernetes provisioning with kubeadm
- Cloud-native networking with Cilium and Gateway API
- GitOps delivery with Argo CD
- Centralized secrets management with HashiCorp Vault
- Monitoring, logging, and network observability
- Stateful workloads in Kubernetes
- Separation of source control, CI, and deployment responsibilities

## Infrastructure Layer

The infrastructure runs on a single physical server.

### Physical Host
- **Platform:** Proxmox VE 9
- **Purpose:** virtualization layer for all platform virtual machines

### Virtual Machines
- **k8s-cp1** — Kubernetes control plane node
- **k8s-w1** — Kubernetes worker node
- **k8s-w2** — Kubernetes worker node

Terraform is used to provision the virtual machines in Proxmox.  
Ansible is used to configure the operating systems and prepare the Kubernetes nodes.

The self-hosted GitLab Runner is deployed inside Kubernetes after the cluster is available, not as a standalone VM.

## Kubernetes Layer

The runtime platform is a kubeadm-based Kubernetes cluster with:

- **1 control plane**
- **2 worker nodes**

The cluster hosts platform services, CI runner workloads, and application workloads.

### Namespace Layout

| Namespace | Purpose |
|---|---|
| `platform` | Argo CD, Prometheus, Grafana, Loki, Alertmanager, cert-manager |
| `ci` | GitLab Runner |
| `security` | Vault |
| `messaging` | NATS JetStream |
| `data` | PostgreSQL, Redis |
| `apps` | frontend, api, worker |

## Networking Layer

The Kubernetes networking stack is based on **Cilium**.

At the infrastructure layer, the initial deployment assumes a single public IP on the Proxmox host. Kubernetes nodes live on a private Proxmox bridge behind NAT, and external access is published through the host as controlled port forwarding or gateway exposure.

### Main components
- **Cilium CNI** — cluster networking
- **Gateway API** — north-south traffic entry point
- **Hubble** — network observability

### Traffic flow
External traffic enters the platform through the Kubernetes gateway and is routed to services running inside the cluster.

Example flow:

`User → Gateway → frontend → api`

### External access model

- Proxmox host keeps the public IP address
- Kubernetes nodes use a private subnet on a dedicated Proxmox bridge
- future external `kubectl` access can be exposed through the host public IP and TCP forwarding to the control plane
- future application access can be exposed through the same public IP and Kubernetes gateway listeners on `80/443`

## GitOps Layer

The deployment model follows a GitOps approach.

### Components
- **GitHub** — source code repositories
- **GitHub** — GitOps manifests repository
- **Argo CD** — GitOps controller inside Kubernetes

### Process
- application code is stored in GitHub
- CI builds container images
- CI updates the GitOps repository
- Argo CD detects manifest changes
- Argo CD synchronizes the cluster state

This ensures that the desired cluster state is managed declaratively in Git.

## CI Layer

Continuous Integration is performed with **GitLab CI (SaaS)**.

### Components
- **GitLab CI** — pipeline orchestration
- **GitLab Runner deployment in Kubernetes** — self-hosted runner for GitLab CI jobs
- **Container registry** — stores built images

### Responsibilities
- run lint and tests
- build container images
- run security checks
- push images to the registry
- update GitOps manifests

GitLab CI orchestrates the pipelines, while the runner executes jobs from inside the Kubernetes cluster.

The CI pipeline does not deploy directly to Kubernetes.  
Deployment is performed indirectly through GitOps.

## Security Layer

Secrets are managed through **HashiCorp Vault**.

### Integration model
- Vault runs inside Kubernetes
- Kubernetes workloads authenticate to Vault
- Secrets are injected into application pods at runtime

Examples of managed secrets:
- database credentials
- API keys
- service tokens

## Observability Layer

The platform includes a full observability stack.

### Components
- **Prometheus** — metrics collection
- **Grafana** — dashboards and visualization
- **Loki** — centralized logging
- **Hubble** — network flow visibility

### Coverage
- cluster health
- node health
- application behavior
- logs
- network communication

## Data and Messaging Layer

The platform includes the following stateful services:

### Data services
- **PostgreSQL** — primary relational database
- **Redis** — in-memory cache

### Messaging service
- **NATS JetStream** — asynchronous communication and background job dispatching

These services support the application runtime and demonstrate platform support for stateful workloads.

## Application Layer

The application layer contains three components:

- **frontend**
- **api**
- **worker**

### Runtime flow
- frontend sends requests to api
- api reads and writes application data
- api uses Redis for caching
- api publishes events to NATS
- worker consumes events and processes background jobs
- secrets are delivered from Vault

## End-to-End Delivery Flow

The full delivery flow is:

1. Developer pushes code to GitHub
2. GitLab CI starts a pipeline
3. The self-hosted GitLab Runner deployment inside Kubernetes executes the jobs
4. A new container image is built and pushed to the registry
5. The GitOps repository is updated with the new image version
6. Argo CD detects the change
7. Argo CD synchronizes the new desired state into Kubernetes
8. Kubernetes performs a rolling update of the workload

## Scope

This repository documents the platform architecture, delivery model, and implementation planning.

It serves as the top-level documentation layer for the overall self-hosted platform.
