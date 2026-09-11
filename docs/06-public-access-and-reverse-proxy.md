# Public access and reverse proxy

## Access layers

The current homelab uses more than one access layer:

```text
LAN client
  -> local DNS / direct IP / local reverse proxy
  -> service

Internet client
  -> Cloudflare
  -> Cloudflare Tunnel
  -> internal service or reverse proxy
```

## Nginx Proxy Manager

Nginx Proxy Manager is used for local reverse proxy routing and convenient hostnames.

Good uses:

- Local-only service names
- HTTPS termination for LAN apps
- Proxying apps that support reverse proxy headers

Avoid using it to expose privileged admin interfaces publicly unless strong authentication and access controls are in place.

## Cloudflare Tunnel

Cloudflare Tunnel is used for selected public-facing services.

Good candidates:

- Nextcloud, with proper trusted domain and overwrite settings
- n8n, if intentionally exposed and protected
- Public dashboards only if access-controlled

Poor candidates:

- Proxmox admin UI
- Portainer
- CasaOS
- Tugtainer
- Docker socket related tools
- Unprotected databases
- SMB/NFS

## Cloudflare security checklist

- Use Zero Trust / Access policies for sensitive apps.
- Keep tunnel tokens out of git.
- Do not paste full `docker run cloudflared ... --token ...` commands into public docs.
- Document the service name and purpose, not the secret token.
- Verify each hostname maps only to the intended internal service.

## Reverse proxy app notes

Some apps need special reverse proxy settings:

- Nextcloud AIO needs correct trusted domains, overwrite host, protocol, and app URLs.
- WebSocket-heavy apps need WebSocket support enabled.
- Apps with absolute URLs may need a configured public base URL.
- Apps with large uploads may need body-size/time-out tuning.

## Public exposure rule

If a service can control containers, hosts, DNS, storage, backups, or secrets, keep it private by default.
