# Docker host and service catalog

## Service map

<p align="center">
  <img src="../assets/service-map.svg" alt="Service map" width="100%">
</p>
## Main Docker host

Most applications run inside the main Ubuntu LXC as Docker containers.

Management tools used in the current stack:

- Docker / Docker Compose
- CasaOS
- Portainer
- Tugtainer
- Nginx Proxy Manager

## Service catalog

### Infrastructure and management

```text
CasaOS                  Host/app dashboard
Portainer               Docker management
Tugtainer               Container update visibility/management
Nginx Proxy Manager     Local reverse proxy and certificates
Cloudflared             Cloudflare Tunnel connector
Pi-hole                 DNS/ad-blocking service
Guacamole               Browser-based remote access
```

### Monitoring and dashboards

```text
Homarr                  Homelab dashboard
Grafana                 Metrics dashboard
Prometheus              Metrics database/scraper
Node Exporter           Host metrics exporter
Blackbox Exporter       Endpoint probing
Uptime Kuma             Uptime/status monitoring
```

### Media and downloads

```text
Plex                    Media server
Sonarr                  TV automation
Bazarr                  Subtitle automation
qBittorrent             Torrent client
JDownloader             Download manager
Social video downloader Custom yt-dlp based downloader
Telegram media downloader Telegram media archive/downloader
```

### Cloud, photos, and personal apps

```text
Nextcloud AIO           Private cloud/files/collaboration
Immich                  Photo/video backup and gallery
Vaultwarden             Password manager
n8n                     Automation workflows
OrderVault              Custom order/renewal tracker
```

## Current operating style

- Keep app state in persistent volumes or bind-mounted paths.
- Back up compose files and app data before edits.
- Recreate only the target container when possible.
- Verify app health after restart.
- Avoid global auto-updates for critical stateful apps.

## Apps that need extra caution

Be careful with automatic updates or blind recreation for:

- Nextcloud AIO
- Immich and its Postgres/Redis services
- n8n
- Vaultwarden
- Plex
- Pi-hole
- Databases
- Reverse proxy / tunnel components

For these, prefer:

1. Version discovery
2. Backup
3. Changelog/security review
4. Targeted update
5. Health and UI/API verification
6. Rollback plan

## Example read-only inventory commands

```bash
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}'
docker compose ls
systemctl --type=service --state=running
ss -ltnp
```

Never paste secrets from `docker inspect`, `.env`, compose files, or application configs into public documentation.
