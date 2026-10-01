# Current deployment and verification baseline

[Portfolio](../README.md) · [Change history](../CHANGELOG.md) · [Employer guide](for-employers.md)

**Documentation baseline: October 1, 2026.** Each row identifies the latest relevant observation. A historical screenshot, a completed scrub date, and the date a status command was run are different records.

## Hosts and workloads

| Component | Recorded deployment | Most recent evidence represented here |
|---|---|---|
| Compute host | Lenovo Legion Y540; Proxmox `pve`; LXC workloads | August screenshots display Proxmox VE 9.2.10; later container commands confirm operation |
| Storage host | Lenovo IdeaPad Y700; TrueNAS; single-disk ZFS pool `tank` | September 28 status: ONLINE, zero READ/WRITE/CKSUM counters, no known data errors |
| CT100 | Immich, Docker Compose, PostgreSQL, Redis, machine learning | October 1 Compose state and application confirmation |
| CT101 | Cloudflare Tunnel | August resource/dashboard screenshots; no later external-route result retained |
| CT102 | Homepage and `homelab-immich-updater` | September 30 updater service active/enabled; October 1 update and dashboard verification |

## Immich services and images

| Container | Recorded image or behavior | October 1 state |
|---|---|---|
| `immich_server` | Release image; port 2283 published | Running and healthy |
| `immich_machine_learning` | `ghcr.io/immich-app/immich-machine-learning:release` | Running and healthy |
| `immich_postgres` | `ghcr.io/immich-app/postgres:14-vectorchord0.4.3-pgvectors0.2.0` | Running and healthy |
| `immich_redis` | `redis:6.2-alpine` | Running |

Release tags are mutable, so the record does not assign an exact application version or digest to the October 1 update. PostgreSQL's recorded image tag is retained as the compatibility baseline, not a recommendation to upgrade its major version automatically.

## Paths and operational ownership

| Location | Purpose | Owner/layer |
|---|---|---|
| `/mnt/tank/restored` | Restored data dataset; September 28 `USED` value 133G | TrueNAS |
| `/mnt/pve/immich-nfs` | Host NFS mount | Proxmox |
| `/mnt/immich-data` | Bind mount of NAS media inside CT100 | Immich LXC |
| `/opt/immich/docker-compose.yml` | Running application Compose configuration | CT100 |
| Local Linux/container database storage | PostgreSQL data, separate from NAS media | CT100 |
| `/opt/homelab/ops/app.py` | Python operations web service | CT102 |
| `/usr/local/sbin/immich-update` | Forced update command and locking logic; invokes `pct exec 100` | Proxmox `pve` host |

The current private `.env`, database dump contents, SSH keys, API credentials, and personal media are excluded. The exact active database host directory is not reconstructed from an older Windows path.

## Status of security and recovery work

| Work | Recorded state |
|---|---|
| Encrypted local Restic repositories | Implemented; checks/copy/test restore recorded |
| Manual PostgreSQL logical backup | Original preparation file recorded on August 14 |
| Proxmox Homepage API credential | Regenerated; widget checked after Homepage restart on October 1 |
| Updater execution restriction | Forced-command/no-forwarding configuration installed; permitted update path exercised |
| Updater locking | Locking present in script; separate contention test not recorded |
| Updater network exposure | Listener recorded on the operations node's home-LAN address, port 8081; instruction is to keep it off Cloudflare/public access |
| Dataset least privilege | Broad recovery ACL remains an improvement item |
| Offsite backups and automated retention | Planned/not demonstrated |

## Nextcloud and planned components

Nextcloud ran in the original Windows environment. Migration to the new Proxmox/TrueNAS platform remains planned in this portfolio. VLANs, OPNsense, WireGuard, SIEM/centralized logging, vulnerability scanning, and an offsite destination are also planned.

See the [validation record](../evidence/validation-record.md) for the underlying dated results and [change history](../CHANGELOG.md) for how this baseline evolved.
