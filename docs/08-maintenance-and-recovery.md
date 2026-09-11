# Maintenance and recovery notes

## General maintenance workflow

For service changes:

1. Identify the exact service/container/compose path.
2. Back up compose/config/app data first.
3. Make one change at a time.
4. Recreate or restart only the target service when possible.
5. Verify container health, HTTP/API response, and UI behavior.
6. Keep rollback files until the replacement is proven.

## Safe Docker update workflow

```bash
# Read-only discovery first
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}'
docker inspect <container>

# Back up compose and app data before editing
# Pull/update only the intended image/service
# Recreate only the affected service
# Verify health and logs
```

Avoid blind `latest` updates for critical services.

## Services requiring careful updates

- Nextcloud AIO
- Immich
- n8n
- Vaultwarden
- Plex
- Pi-hole
- Nginx Proxy Manager
- Cloudflared
- Any database-backed app

## Proxmox/storage change workflow

Before disk, ZFS, or Proxmox storage changes:

1. Run full discovery.
2. Identify disks by model/serial and `/dev/disk/by-id/`.
3. Confirm pool topology.
4. Back up Proxmox configs.
5. Ask for explicit approval before destructive commands.
6. Validate pool, storage, guests, and services afterward.

## Recovery checklist

If a service is down:

1. Check whether the host is reachable.
2. Check whether Docker is running.
3. Check the container status and health.
4. Check logs for only the affected service first.
5. Check disk space.
6. Check reverse proxy/tunnel routing if the LAN service works but public URL fails.
7. Check app-specific database health for apps like Immich, Nextcloud, and Vaultwarden.

## Rollback principles

- Do not delete old containers/configs immediately after a repair.
- Disable restart on old rollback containers if they conflict with the live service.
- Keep backup folders with timestamps.
- Document what changed and how to undo it.

## Security hygiene

Never commit:

- `.env` files with secrets
- Cloudflare tunnel tokens
- GitHub tokens
- Proxmox API tokens
- Passwords
- Private SSH keys
- App encryption keys
- Database dumps with private data
- Full `docker inspect` output without redaction

Use placeholders in docs:

```text
<LAN_IP>
<DOMAIN>
<API_TOKEN>
<SECRET>
<PASSWORD>
```
