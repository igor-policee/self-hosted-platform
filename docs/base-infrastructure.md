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

## Remaining Step 1 Work

- Prepare a reusable cloud-init VM template in Proxmox
- Confirm the guest operating system choice for Kubernetes nodes
- Record the template name and image source for later Terraform inputs
