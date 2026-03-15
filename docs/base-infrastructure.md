# Base Infrastructure

This document records the intended Step 1 infrastructure baseline for the platform.

It is not a dump of live host configuration. It captures the target shape that later automation and operations should reproduce.

## Current Host

- Proxmox host name: `Einstein`
- Proxmox public IP: `95.213.143.83/24`
- Proxmox default gateway: `95.213.143.1`
- Proxmox DNS resolvers: `188.93.17.19`, `188.93.16.19`
- Primary public interface: `eth0`
- Primary local storage: `local`

## Step 1 Network Baseline

The initial deployment assumes a single public IP on the Proxmox host.

The host keeps the public IP on `eth0`. Kubernetes virtual machines use a private bridge and reach the internet through host-side NAT.

### Proxmox Bridges

| Bridge | Purpose | Addressing | Notes |
|---|---|---|---|
| `eth0` | Proxmox host public uplink | `95.213.143.83/24` | Management and external entry point |
| `vmbr1` | Private VM network | `10.20.0.1/24` | Dedicated bridge for Kubernetes VMs |

### NAT Model

- Host-side IPv4 forwarding is enabled
- Outbound traffic from `10.20.0.0/24` is masqueraded through `eth0`
- The public IP remains attached to the Proxmox host
- Future external access to Kubernetes is expected to use controlled port forwarding or gateway exposure through the host

## Planned VM Addressing

| VM | Role | IP address |
|---|---|---|
| `k8s-cp1` | Kubernetes control plane | `10.20.0.10/24` |
| `k8s-w1` | Kubernetes worker | `10.20.0.11/24` |
| `k8s-w2` | Kubernetes worker | `10.20.0.12/24` |

Shared VM network settings:

- Gateway: `10.20.0.1`
- DNS: `188.93.17.19`, `188.93.16.19`
- Network bridge: `vmbr1`

## Future External Access

- Kubernetes API can later be exposed through the Proxmox host public IP and TCP forwarding to `k8s-cp1:6443`
- Application traffic can later be exposed through the same public IP with Kubernetes gateway listeners on `80/443`

## VM Template Baseline

- Template VMID: `9000`
- Template name: `debian-12-bookworm-cloudinit`
- Guest OS baseline: Debian 12 Bookworm generic cloud image
- Source image URL: `https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-generic-amd64.qcow2`
- Template network bridge: `vmbr1`
- Default cloud-init user: `devops`

The template is intended for cloning the Kubernetes control plane and worker nodes during later Terraform provisioning.

## Step 1 Completion Status

Step 1 is complete for the current platform baseline.

Delivered outcomes:

- Proxmox VE host prepared and reachable
- SSH hardening applied for key-based administration
- Separate Proxmox Web UI and Terraform automation identities created
- Private Kubernetes VM network established on `vmbr1`
- Host-side NAT enabled for `10.20.0.0/24`
- Reusable Debian 12 cloud-init VM template prepared as `debian-12-bookworm-cloudinit`

The next implementation step is Step 2, where the Terraform automation layer will be rebuilt against this baseline.
