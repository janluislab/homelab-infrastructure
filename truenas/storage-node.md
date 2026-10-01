# Storage node — Lenovo IdeaPad Y700

[Back to the portfolio](../README.md)

The Y700 provides dedicated TrueNAS storage to the Proxmox compute host. Build notes record **TrueNAS Community Edition 25.10**, a ZFS pool named `tank`, and NFS/SMB sharing.

## Hardware and storage

| Component | Recorded specification |
|---|---|
| System | Lenovo IdeaPad Y700 15ISK |
| CPU | Intel Core i7-6700HQ |
| GPU | NVIDIA GTX 960M |
| RAM | Build notes list up to 16 GB; exact installed capacity not captured here |
| Data disk | External 4 TB HDD; later diagnostics identify Seagate ST4000NT001-3M2101 |
| Pool | `tank`, one data disk |
| Restored data | `/mnt/tank/restored` |

## Sharing and application boundaries

NFS exports the restored media to Proxmox. The host mounts the share at `/mnt/pve/immich-nfs`, then exposes it inside CT100 at `/mnt/immich-data`. SMB was also configured during the migration.

Application database files stay on local Linux/container storage. Media backup and database backup are separate recovery requirements.

The [NFS incident report](../documentation/incident-postmortems/nfs-permissions.md) explains export identity mapping, group ownership, recursive dataset ACLs, and unprivileged LXC UID mapping involved in restoring write access.

## Integrity evidence and limits

During the August 27, 2026 investigation, recorded results included SMART passing with zero reallocated, pending, uncorrectable, and interface CRC counts; a clean later scrub; and three sample file hashes matching copies restored from Restic.

A later August interruption included metadata errors. A subsequent **September 28, 2026** status recorded `tank` ONLINE, READ/WRITE/CKSUM counters at zero, and no known data errors. It displayed a completed **September 13** scrub: 0B repaired, zero errors, duration 01:14:22. The dataset `tank/restored` was mounted at `/mnt/tank/restored`, with 133G used.

This later result documents the clean recovery state. The exact physical cause and intermediate repair action are still not established by the retained record.

The [storage incident report](../documentation/incident-postmortems/truenas-storage-hdd.md) and [validation record](../evidence/validation-record.md) preserve both the original error observations and the later clean checkpoint.

## Accepted lab constraints

The pool has **no mirrored data disk or RAIDZ redundancy**. ZFS can detect checksum problems, but this layout has no redundant data disk from which to reconstruct damaged file contents. Snapshots on the same disk do not replace an independent backup.

The external connection and disk are additional dependencies. A broad ACL used during recovery is documented as a hardening gap, not least-privilege access.

## Read-only checks for future validation

Run on TrueNAS:

```bash
sudo zpool status -v tank
sudo zpool list tank
sudo zfs list -r tank
```

Review pool state, device counters, scrub results, and the permanent-error section together. `ONLINE` alone does not mean that the pool reports no data errors.

See [backup and recovery](../backup/restic-strategy.md) for recovery copies and the [operations runbook](../documentation/operations-runbook.md) for dependency checks.
