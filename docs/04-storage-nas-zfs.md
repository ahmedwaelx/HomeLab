# Storage, NAS, and ZFS

## Current storage model

The current homelab uses ZFS-backed storage instead of only Proxmox local storage.

Current storage tiers:

```text
Main ZFS pool
  Purpose: active NAS/application data and primary backup hub

Secondary ZFS pool
  Purpose: mini-node backup tier
```

## NAS pattern

The main Ubuntu LXC receives selected ZFS-backed paths from Proxmox as bind mounts. Inside the LXC, services expose those paths through:

- SMB/Samba
- NFS
- Docker bind mounts
- Application-specific volumes

This keeps storage management at the Proxmox/ZFS layer while application services run inside the LXC.

## Common dataset categories

Typical dataset categories for this homelab:

```text
media       General media library
plex        Plex-specific library data
photos      Photo libraries and Immich-related data
nextcloud   Nextcloud data/external storage
shared      General SMB/NFS shared files
downloads   Downloader targets
docker      App data and Docker-related persistent state
backups     Local backup destination
```

Exact dataset names and mount paths should be confirmed on the live host before edits.

## SMB/NFS services

The Ubuntu LXC runs Samba and NFS services for LAN access.

General principles:

- Keep guest access disabled unless explicitly needed.
- Use named users/groups for authenticated shares.
- Keep write access scoped to the LAN.
- Validate SMB from both inside the LXC and from a LAN client.
- Avoid exposing SMB/NFS outside the LAN.

## ZFS maintenance

Recommended maintenance tasks:

- Scheduled ZFS scrubs
- SMART monitoring for disks
- Snapshot policy for important datasets
- Backup replication or secondary backup tier
- Periodic restore testing

## Snapshot warning

ZFS snapshots are useful for rollback, but snapshots are not complete backups. If the pool is lost, snapshots on that same pool are lost too.

A practical setup uses:

1. Snapshots for fast local rollback
2. Proxmox backups for VM/CT recovery
3. Secondary local backup pool
4. Optional encrypted offsite backup for critical data
5. Periodic restore tests
