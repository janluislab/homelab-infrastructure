# Operations and recovery runbook

[Back to the portfolio](../README.md)

This is a guide for future checks. Example commands are not evidence of actions executed during the documentation update. Use the existing private configuration; do not paste credentials into command output or public files.

## Dependency order after an outage

| Order | Check | Required observation |
|---|---|---|
| 1 | Home network and host addresses | Management client and hosts have the intended network configuration |
| 2 | TrueNAS storage | Pool status, scrub results, and permanent-error details reviewed together |
| 3 | Proxmox NFS mount | The media path is actually mounted from NFS, not an empty local fallback directory |
| 4 | CT100 bind mount and local database | Expected media path and database storage are present |
| 5 | Immich Compose services | Required services running; service health and logs checked |
| 6 | Application behavior | Login, existing photos, representative video, and an intended test upload work |
| 7 | Cloudflare/Homepage | External route and dashboard checked after local application health |

Do not start media-writing services against an unmounted NFS path: files could be written to the host's local directory instead of the intended NAS.

## TrueNAS checks

Run on TrueNAS:

```bash
sudo zpool status -v tank
sudo zpool list tank
sudo zfs list -r tank
```

Record pool state, device READ/WRITE/CKSUM counters, scrub completion, and any permanent-error paths. An `ONLINE` label or passing SMART assessment alone does not prove that all data is healthy. If errors recur, preserve output and validate backup recovery before choosing a repair action.

## Proxmox checks

Run on the Proxmox host:

```bash
ip -br address
ip route
systemctl is-active pveproxy pvedaemon
pct list
findmnt -T /mnt/pve/immich-nfs
pct config 100
```

For the media path, confirm the expected remote source and an NFS filesystem type. `findmnt -T` may return the surrounding local filesystem if the desired mount is missing; command success alone is not sufficient.

If the management UI is unavailable but ping responds, inspect the management services and recent logs rather than treating ping as proof that Proxmox is fully recovered:

```bash
journalctl -b -u pveproxy -u pvedaemon --no-pager -n 80
```

## Immich checks

Run on the Proxmox host:

```bash
pct exec 100 -- findmnt -T /mnt/immich-data
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml ps
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml logs --tail 80 immich-server
```

Inspect the actual Compose service names if the configuration differs. After storage is verified, use the existing deployment workflow to start the stack. A standard Compose start command for this recorded location is:

```bash
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml up -d
```

A healthy container is one check. Also open the application, verify media playback, and check representative library content. Do not delete database directories or recreate a pool as a generic recovery step.

## Backups and change records

Before a planned application update, verify a usable recovery copy of media, an application-consistent database backup, and the private configuration. Record the previous version, change, result, and recovery method. The project's updater completed successfully on October 1, 2026. Its deployed service, paths, restricted execution, locking, and validation flow are recorded in the [operations-node page](../proxmox/operations-node.md); an automatic pre-update backup is not demonstrated.

Use the [Restic guide](../backup/restic-strategy.md) for repository and test-restore checks. Keep full logs private, and publish a sanitized, dated validation summary. Update [current deployment](current-state.md) and [change history](../CHANGELOG.md) after a verified change; the [documentation workflow](documentation-workflow.md) lists the record fields.
