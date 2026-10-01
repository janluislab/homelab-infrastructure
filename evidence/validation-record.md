# Recorded validation results

[Evidence index](README.md) · [Current deployment](../documentation/current-state.md) · [Change history](../CHANGELOG.md)

Dated terminal results and owner-confirmed outcomes are summarized below. Values are kept within their original scope; this page is a validation record, not a newly executed live audit or an invented full transcript.

## August 14, 2026 — migration preparation

| Check | Recorded result | Scope |
|---|---|---|
| Windows library inventory | 22,118 files; 38.24 GB | Immich library at that checkpoint |
| Source drive inventory | Roughly 526 GB used | Entire `E:` drive; not the library size or proof of a full completed transfer |
| Manual PostgreSQL backup | `immich-manual-backup.sql.gz`, 56,789,824 bytes | Backup file produced; a restore of that exact SQL dump is not separately demonstrated |
| Database image change | VectorChord-compatible PostgreSQL image; database healthy, existing data retained; vector initialization/index work in logs | Compatibility blocker addressed; subsequent connection/import failure recorded separately |

Details: [migration preparation](../documentation/migration-preparation.md) and [vector-extension case study](../documentation/incident-postmortems/immich-vector-extension.md).

## August 26–27, 2026 — storage investigation

| Check | Recorded result | Scope |
|---|---|---|
| SMART | Overall assessment passed; reallocated, pending, uncorrectable, and interface CRC counts zero | Device observation at that time |
| Restic repository check | No errors reported | Default check; full `--read-data` coverage not demonstrated |
| Separate test restore | 38.244 GiB; 24:51 duration; approximately 37.9 thousand files/directories in the progress/result capture | Tested restore, distinct from regular-file inventory or all source-drive usage |
| Later ZFS scrub/status | `tank` ONLINE; 0B repaired; zero scrub errors; no known data errors | Historical result for that episode |
| SHA-256 comparison | Three flagged JPG/MP4 files matched their Restic-restored copies | Three sampled files, not all media |

The initial error list contained 3,878 data-error entries; deduplication identified 3,827 unique paths, including 3,808 media and 19 other paths. Error counts and unique-path counts measure different things.

## August 31, 2026 — storage recurrence

The record included a suspended-pool observation followed by an ONLINE pool with a scrub in progress, 5,216 data errors, and metadata entries in the permanent-error section. The exact physical cause and intermediate repair actions are not established by that record.

The later clean storage observation below supersedes the earlier portfolio summary's missing final-status detail.

## September 28, 2026 — clean storage checkpoint

| Check | Recorded result |
|---|---|
| Pool state | `tank` ONLINE |
| Device error counters | READ/WRITE/CKSUM 0/0/0 |
| Permanent-error section | No known data errors |
| Completed scrub displayed | September 13, 2026; 0B repaired; zero errors; duration 01:14:22 |
| Restored dataset | `tank/restored` mounted at `/mnt/tank/restored`, USED 133G |
| Pool usage | `tank` USED 135G |

The status output was collected on **September 28**. The completed scrub displayed in it was dated **September 13**. This documents the later clean pool state without assigning an unsupported hardware cause to the preceding incident.

## September 30, 2026 — operations-node implementation

| Check | Recorded result | Boundary |
|---|---|---|
| systemd service | `homelab-immich-updater` active/running and enabled | Service state in the captured result |
| Program | `/usr/bin/python3 /opt/homelab/ops/app.py` | Installed service execution path |
| Listener | Operations node home-LAN address, port 8081 | Listener configuration; full external-exposure audit not recorded |
| Restricted SSH | Intended update command succeeded with forced-command/no-forwarding configuration installed | Arbitrary-command rejection tests not recorded |
| Update locking | Locking lines present in `/usr/local/sbin/immich-update` | Separate contention test not recorded |

Details: [operations node](../proxmox/operations-node.md).

## October 1, 2026 — successful update and application verification

The manual update log recorded image pulling, container recreation, a wait for readiness, API health success, and completion. The subsequent command was:

```bash
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml ps
```

| Container | Recorded state |
|---|---|
| `immich_server` | Running and healthy; port 2283 published |
| `immich_machine_learning` | Running and healthy |
| `immich_postgres` | Running and healthy; VectorChord-compatible PostgreSQL 14 image |
| `immich_redis` | Running; `redis:6.2-alpine` |

The owner confirmed that everything was working after this check. No exact release digest or measured recovery-time target is inferred from the result.

## October 1, 2026 — credential and dashboard follow-up

The **Proxmox API credential used by Homepage** was regenerated. Homepage was restarted in CT102, and its Proxmox integration was confirmed working. This identifies the completed rotation accurately; completion of other service-key rotations was not recorded.

Credential values, earlier credential-bearing configuration, private service URLs, database dump contents, and personal media remain excluded.

See [application recovery](../documentation/incident-postmortems/application-recovery.md), [storage investigation](../documentation/incident-postmortems/truenas-storage-hdd.md), and [security controls](../security/controls-and-limitations.md).
