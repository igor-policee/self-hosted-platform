# GitHub Issues Plan

This document defines the recommended high-level GitHub Issues for implementing the Self-Hosted DevOps Platform.

---

## Issue 1 — Build the base infrastructure

**Description:**  
Prepare the physical server, install Proxmox VE, configure storage and networking, and create the base VM template.

**Done when:**
- Proxmox VE is installed and reachable
- base networking is configured
- storage is configured
- VM template is ready for cloning

---

## Issue 2 — Build the infrastructure automation

**Description:**  
Create the Terraform configuration for Proxmox so that the virtual machine infrastructure can be provisioned in a reproducible way.

**Done when:**
- Terraform project structure exists
- Proxmox provider is configured
- VM definitions are prepared
- infrastructure code is ready to provision the platform VMs

---

## Issue 3 — Provision the core virtual machines

**Description:**  
Use Terraform to create the main virtual machines required for the platform.

**Done when:**
- `gitlab-runner` is created
- `k8s-cp1` is created
- `k8s-w1` is created
- `k8s-w2` is created
- all VMs are reachable over SSH
- infrastructure can be re-applied consistently

---

## Issue 4 — Build the configuration automation

**Description:**  
Create the Ansible configuration to prepare all virtual machines, apply baseline system configuration, and install Kubernetes prerequisites.

**Done when:**
- Ansible inventory is ready
- baseline OS configuration is applied
- container runtime is installed
- Kubernetes prerequisites are installed
- nodes are ready for cluster bootstrap

---

## Issue 5 — Build the Kubernetes platform

**Description:**  
Bootstrap the Kubernetes cluster with one control plane and two worker nodes.

**Done when:**
- control plane is initialized
- worker nodes are joined
- `kubectl get nodes` shows all nodes as ready
- baseline cluster services are installed

---

## Issue 6 — Build the networking platform

**Description:**  
Install and configure Cilium as the cluster networking layer and enable Gateway API for ingress traffic.

**Done when:**
- Cilium is installed
- pod-to-pod networking works
- service networking works
- Gateway API is configured
- external traffic can reach the cluster

---

## Issue 7 — Configure DNS and TLS

**Description:**  
Set up DNS records and TLS certificates for platform and application access.

**Done when:**
- domain names resolve correctly
- TLS certificates are issued
- HTTPS access works for exposed services

---

## Issue 8 — Build the GitOps platform

**Description:**  
Create the GitOps repository and deploy Argo CD to manage cluster state from Git.

**Done when:**
- GitOps repository exists
- Argo CD is installed
- Argo CD can sync cluster resources from Git
- Git is the source of truth for deployment state

---

## Issue 9 — Build the observability platform

**Description:**  
Deploy monitoring, logging, and network observability components for the platform.

**Done when:**
- Prometheus is running
- Grafana is running
- Loki is running
- Hubble is enabled
- basic dashboards are available

---

## Issue 10 — Build the secrets platform

**Description:**  
Deploy HashiCorp Vault and integrate it with Kubernetes for secrets delivery.

**Done when:**
- Vault is deployed
- Kubernetes authentication is configured
- workloads can receive secrets from Vault

---

## Issue 11 — Build the data platform

**Description:**  
Deploy the core stateful services used by applications: PostgreSQL, Redis, and NATS JetStream.

**Done when:**
- PostgreSQL is available
- Redis is available
- NATS JetStream is available
- applications can connect to them

---

## Issue 12 — Build the application platform

**Description:**  
Deploy the demo application components: frontend, api, and worker.

**Done when:**
- `frontend` is deployed
- `api` is deployed
- `worker` is deployed
- application routing works
- services communicate correctly

---

## Issue 13 — Build the CI platform

**Description:**  
Configure GitLab CI and the self-hosted GitLab Runner to build images and update the GitOps repository.

**Done when:**
- GitLab Runner is registered and working
- CI pipeline builds application images
- images are pushed to the registry
- GitOps manifests are updated automatically

---

## Issue 14 — Build the operations layer

**Description:**  
Add access control, network policies, backup procedures, upgrade procedures, and final technical documentation.

**Done when:**
- RBAC is configured
- network policies are applied
- backup and restore procedures are documented
- upgrade process is documented
- project documentation is complete

---

## Recommended Issue Order

1. Build the base infrastructure  
2. Build the infrastructure automation  
3. Provision the core virtual machines  
4. Build the configuration automation  
5. Build the Kubernetes platform  
6. Build the networking platform  
7. Configure DNS and TLS  
8. Build the GitOps platform  
9. Build the observability platform  
10. Build the secrets platform  
11. Build the data platform  
12. Build the application platform  
13. Build the CI platform  
14. Build the operations layer
