# Network and access model

## LAN model

The homelab uses private LAN addressing for Proxmox nodes, the Docker/NAS LXC, and internal dashboards.

Example private layout:

```text
Main Proxmox node      192.168.1.x
Mini Proxmox node      192.168.1.x
Ubuntu Docker/NAS LXC  192.168.1.x
```

Keep exact credentials, SSH keys, API tokens, and provider secrets outside this repository.

## Access principles

1. Keep admin surfaces LAN-only unless there is a strong reason.
2. Use Cloudflare Tunnel for selected public apps, not for everything.
3. Put public apps behind authentication where possible.
4. Do not expose Proxmox, Portainer, CasaOS, Tugtainer, or Docker socket management directly to the Internet.
5. Prefer local DNS/reverse proxy names for LAN convenience.

## Common local ports

These are examples from the current service layout. Confirm against the live host before relying on them.

```text
Proxmox VE                  8006
CasaOS                      8081
Portainer                   9443 / 9000
Tugtainer                   9412
Nginx Proxy Manager         80 / 81 / 443
Grafana                     3003
Prometheus                  9090
Uptime Kuma                 3001
Plex                        32400
Immich                      2283
n8n                         5678
Vaultwarden                 10380
Sonarr                      8989
Bazarr                      6767
qBittorrent                 8181
Pi-hole                     8800 / DNS port mapping
Guacamole                   8090
Social video downloader     8095
Telegram media downloader   8096
Backup alert relay          8098
```

## Public access

Public access is handled through Cloudflare Tunnel for selected services. A public hostname should map to only the intended internal service, with no unnecessary admin panels exposed.

Examples of public-service categories:

- Nextcloud
- n8n webhooks/UI where intentionally exposed
- Selected dashboards only if protected

## VPN

The original guide used Tailscale as the easiest VPN path. It is still a good option for private admin access, but the current stack mainly documents LAN access plus selected Cloudflare Tunnel exposure.

If VPN access is used:

- Keep it for admin/private services.
- Require account MFA.
- Avoid mixing public tunnels and VPN assumptions in the same service documentation.
