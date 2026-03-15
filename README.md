# Homelab DevOps Platform

Production-style self-hosted DevOps platform running on a single bare-metal server with **Proxmox VE** as the virtualization layer.

## Overview

This project is a portfolio-grade homelab platform designed to demonstrate modern DevOps and Platform Engineering practices in a self-hosted environment.

The platform combines infrastructure automation, Kubernetes operations, GitOps delivery, secrets management, observability, and stateful services into a single cohesive system.

## Goals

This platform is designed to demonstrate:

- Infrastructure as Code with Terraform
- Configuration management with Ansible
- Kubernetes cluster provisioning with kubeadm
- Modern networking with Cilium and Gateway API
- GitOps delivery with Argo CD
- Secrets management with HashiCorp Vault
- Observability with Prometheus, Loki, Grafana, and Hubble
- Stateful services with PostgreSQL, Redis, and NATS JetStream
- CI with external GitLab CI and source of truth in GitHub

## High-Level Architecture

- **Bare-metal host:** Proxmox VE 9
- **VM provisioning:** Terraform
- **VM configuration:** Ansible
- **Kubernetes cluster:** 1 control plane + 2 workers via kubeadm
- **Ingress / north-south traffic:** Cilium Gateway API
- **GitOps:** Argo CD
- **Secrets:** HashiCorp Vault
- **Observability:** Prometheus, Loki, Grafana, Hubble
- **Application runtime:** frontend, api, worker
- **Data layer:** PostgreSQL, Redis
- **Messaging:** NATS JetStream
- **External systems:** GitHub, GitLab CI, container registry

## VM Topology

| VM | Role |
|---|---|
| `gitlab-runner` | Self-hosted runner for external GitLab CI |
| `k8s-cp1` | Kubernetes control plane |
| `k8s-w1` | Kubernetes worker |
| `k8s-w2` | Kubernetes worker |

## Kubernetes Namespaces

| Namespace | Purpose |
|---|---|
| `platform` | Argo CD, Prometheus, Grafana, Loki, Alertmanager, cert-manager |
| `security` | Vault |
| `messaging` | NATS JetStream |
| `data` | PostgreSQL, Redis |
| `apps` | frontend, api, worker |

## Component Placement

| Component | Location |
|---|---|
| Proxmox VE | Bare-metal server |
| gitlab-runner | Proxmox VM |
| k8s-cp1 | Proxmox VM |
| k8s-w1 | Proxmox VM |
| k8s-w2 | Proxmox VM |
| Terraform code | GitHub |
| Ansible code | GitHub |
| Source code repositories | GitHub |
| GitOps repository | GitHub |
| GitLab CI | External SaaS |
| Container registry | External registry |
| Argo CD | Kubernetes |
| Cilium | Kubernetes |
| Gateway API | Kubernetes |
| Prometheus | Kubernetes |
| Grafana | Kubernetes |
| Loki | Kubernetes |
| Hubble | Kubernetes |
| Vault | Kubernetes |
| PostgreSQL | Kubernetes |
| Redis | Kubernetes |
| NATS JetStream | Kubernetes |
| frontend | Kubernetes |
| api | Kubernetes |
| worker | Kubernetes |

## Delivery Flow

1. Code lives in GitHub
2. GitLab CI runs lint, tests, image build, and security checks
3. CI pushes images to the container registry
4. CI updates the GitOps manifests repository
5. Argo CD syncs changes into Kubernetes

## Application Flow

1. `frontend` calls `api`
2. `api` reads and writes to PostgreSQL
3. `api` uses Redis for caching
4. `api` publishes asynchronous events to NATS
5. `worker` consumes NATS events and processes background jobs
6. Application secrets are delivered from Vault

## Documentation

- [Architecture](docs/architecture.md)
- [Implementation Plan](docs/implementation-plan.md)
- [GitHub Issues Plan](docs/github-issues-plan.md)

## Repository Scope

This repository contains the top-level architecture, implementation planning, and project documentation for the platform.
