# Architecture overview

## Visual overview

<p align="center">
  <img src="../assets/homelab-architecture.svg" alt="Visual overview" width="100%">
</p>
## Purpose

This homelab is used for self-hosting, media management, automation, private cloud storage, dashboards, and infrastructure experiments.

The current architecture is built around:

- Proxmox VE as the virtualization layer
- A main Ubuntu LXC as the Docker and NAS service host
- ZFS for durable storage pools and datasets
- Docker/CasaOS/Portainer for application operations
- Cloudflare Tunnel and Nginx Proxy Manager for selected access paths
- Grafana/Prometheus/Uptime Kuma for visibility

## High-level layout

```text
Internet
  |
  | selected public services
  v
Cloudflare Tunnel
  |
  v
Nginx Proxy Manager / service ports
  |
  v
Ubuntu LXC Docker host
  |-- Docker apps
  |-- SMB/NFS NAS services
  |-- Monitoring exporters
  |-- Backup alert relay
  |
  v
ZFS-backed storage on Proxmox

Proxmox cluster
  |-- Main node: storage + CT999 services host
  |-- Mini node: Hermes VM + secondary backup pool
```

## Main components

### Proxmox layer

- Two-node Proxmox setup
- Main node hosts the primary Ubuntu LXC
- Mini node hosts the Hermes VM
- Cluster view provides single-pane management
- HA is intentionally not the main design goal unless a proper quorum/QDevice plan is added

### Storage layer

- Main ZFS pool for active NAS/application data
- Secondary ZFS pool for local backup tier
- Proxmox backup storages are configured for VM/CT recovery
- Bind mounts expose selected datasets into the Ubuntu LXC

### Application layer

Most services run as Docker containers inside the Ubuntu LXC. This keeps app management centralized while Proxmox manages the VM/container and storage layers.

### Access layer

- LAN-first access for admin tools
- Cloudflare Tunnel only for selected public services
- Nginx Proxy Manager for reverse proxy routing
- Avoid exposing privileged admin services directly to the public Internet

### Observability layer

- Grafana dashboards for homelab visibility
- Prometheus for metrics
- Uptime Kuma for endpoint checks
- Backup alert relay for Proxmox backup job notifications
