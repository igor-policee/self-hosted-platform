# Implementation Plan

This document describes the platform implementation in a simple execution order.

It is the main planning document for delivery and can also be used as the source when creating GitHub Issues or milestones.

---

## Step 1 — Build the base infrastructure

At this step, we prepare the environment where the platform will run.

We build:
- [ ] Proxmox VE on the bare-metal server
- [ ] Base networking and storage
- [ ] VM template for future provisioning

Main component locations:
- **Proxmox VE** → bare-metal server
- **VM template** → Proxmox

---

## Step 2 — Build the infrastructure automation

At this step, we create the infrastructure provisioning layer.

We build:
- [ ] Terraform project for Proxmox
- [ ] VM definitions for the platform
- [ ] Reproducible infrastructure provisioning workflow

Main component locations:
- **Terraform code** → GitHub
- **Provisioned virtual machines** → Proxmox

---

## Step 3 — Provision the core virtual machines

At this step, we create the virtual machines required by the platform.

We build:
- [ ] `k8s-cp1`
- [ ] `k8s-w1`
- [ ] `k8s-w2`

Main component locations:
- **All virtual machines** → Proxmox

---

## Step 4 — Build the configuration automation

At this step, we prepare the operating systems and Kubernetes nodes.

We build:
- [ ] Ansible project
- [ ] Baseline VM configuration
- [ ] Container runtime installation
- [ ] Kubernetes prerequisites

Main component locations:
- **Ansible code** → GitHub
- **Configured systems** → Proxmox VMs

---

## Step 5 — Build the Kubernetes platform

At this step, we bootstrap the Kubernetes cluster.

We build:
- [ ] 1 control plane node
- [ ] 2 worker nodes
- [ ] Baseline cluster services

Main component locations:
- **Control plane** → `k8s-cp1`
- **Worker nodes** → `k8s-w1`, `k8s-w2`
- **Cluster workloads** → Kubernetes

---

## Step 6 — Build the networking platform

At this step, we configure internal cluster networking and external traffic routing.

We build:
- [ ] Cilium
- [ ] Gateway API
- [ ] DNS and TLS integration

Main component locations:
- **Cilium** → Kubernetes
- **Gateway API** → Kubernetes

---

## Step 7 — Build the GitOps platform

At this step, we make Git the source of truth for deployments.

We build:
- [ ] GitOps repository
- [ ] Argo CD
- [ ] Git-based deployment workflow

Main component locations:
- **GitOps repository** → GitHub
- **Argo CD** → Kubernetes

---

## Step 8 — Build the observability platform

At this step, we add monitoring, logging, and network visibility.

We build:
- [ ] Prometheus
- [ ] Grafana
- [ ] Loki
- [ ] Hubble

Main component locations:
- **Observability stack** → Kubernetes

---

## Step 9 — Build the secrets platform

At this step, we add centralized secrets management.

We build:
- [ ] Vault
- [ ] Vault integration with Kubernetes
- [ ] Secret delivery to workloads

Main component locations:
- **Vault** → Kubernetes

---

## Step 10 — Build the data platform

At this step, we deploy the core stateful services.

We build:
- [ ] PostgreSQL
- [ ] Redis
- [ ] NATS JetStream

Main component locations:
- **Data and messaging services** → Kubernetes

---

## Step 11 — Build the application platform

At this step, we deploy the application workloads.

We build:
- [ ] frontend
- [ ] api
- [ ] worker
- [ ] application routing and service communication

Main component locations:
- **Application workloads** → Kubernetes

---

## Step 12 — Build the CI platform

At this step, we automate build and delivery.

We build:
- [ ] GitLab CI pipeline
- [ ] Self-hosted GitLab Runner deployment inside Kubernetes
- [ ] Container image build and push workflow
- [ ] GitOps repository update workflow

Main component locations:
- **Source code** → GitHub
- **CI orchestration** → GitLab CI
- **GitLab Runner** → Kubernetes
- **Container images** → container registry

---

## Step 13 — Build the operations layer

At this step, we make the platform maintainable and operationally usable.

We build:
- [ ] access control
- [ ] network policies
- [ ] backup and restore procedures
- [ ] upgrade procedures
- [ ] technical documentation

Main component locations:
- **RBAC and policies** → Kubernetes
- **Backups** → external storage and/or Proxmox backup target
- **Documentation** → GitHub

---

## Recommended Execution Order

- [ ] Build the base infrastructure
- [ ] Build the infrastructure automation
- [ ] Provision the core virtual machines
- [ ] Build the configuration automation
- [ ] Build the Kubernetes platform
- [ ] Build the networking platform
- [ ] Build the GitOps platform
- [ ] Build the observability platform
- [ ] Build the secrets platform
- [ ] Build the data platform
- [ ] Build the application platform
- [ ] Build the CI platform
- [ ] Build the operations layer
