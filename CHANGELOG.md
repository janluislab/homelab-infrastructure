# Infrastructure and documentation change history

[Portfolio](README.md) · [Current deployment](documentation/current-state.md)

This record tracks completed work and dated observations. Entries without an exact execution date are grouped as build-period changes rather than assigned invented dates. Raw private configuration and personal data remain outside the repository.

## October 1, 2026 — operation and verification

- Completed the manual Immich update workflow: image pull, container recreation, wait, API health success, and completion.
- Checked Compose in CT100: server, machine learning, and PostgreSQL healthy; Redis running. Application functionality confirmed.
- Regenerated the Proxmox API credential used by Homepage, restarted Homepage in CT102, and confirmed its integration worked.
- Published the first consolidated documentation update through [PR #1](https://github.com/janluislab/homelab-infrastructure/pull/1).
- Expanded the documentation with migration preparation, vector-extension troubleshooting, the later clean TrueNAS validation, updater implementation details, current-state records, and an employer reading guide. Clarified source-drive usage versus library/test-restore measurements.

Details: [operations node](proxmox/operations-node.md), [application recovery](documentation/incident-postmortems/application-recovery.md), and [validation record](evidence/validation-record.md).

## September 30, 2026 — operations tooling

- Deployed the enabled, running `homelab-immich-updater` service in CT102 using Python and `/opt/homelab/ops/app.py`.
- Confirmed the operations listener on the home-LAN address at port 8081.
- Exercised the restricted SSH update path; the script contained update-locking logic.
- Connected the workflow to the Homepage/Operations Center interface.

Details: [operations node](proxmox/operations-node.md).

## September 28, 2026 — later clean storage observation

- Recorded `tank` ONLINE with zero device READ/WRITE/CKSUM counters and no known data errors.
- The status output displayed a completed September 13 scrub: 0B repaired, zero errors, duration 01:14:22.
- Recorded `tank/restored` mounted at `/mnt/tank/restored`, using 133G; pool `tank` used 135G.

This is the later verification missing from the first portfolio update. The precise physical cause and intermediate storage repair action remain unconfirmed.

## August 31, 2026 — storage recurrence

- Recorded a storage interruption/suspended-pool observation and later in-progress scrub output with metadata error entries.
- Kept this event distinct from the earlier clean August result and the later September status.

Details: [ZFS/HDD investigation](documentation/incident-postmortems/truenas-storage-hdd.md).

## August 26–27, 2026 — integrity investigation and recovery checks

- Investigated 3,878 ZFS data-error entries; identified 3,827 unique paths.
- Reviewed SMART results and a successful Restic repository check.
- Completed a separate 38.244 GiB test restore in 24:51 and compared three suspect media files with their restored copies using SHA-256.
- Recorded a later clean scrub/status for that episode.

## August 20, 2026 — recovery snapshot

- Recorded a Restic snapshot originating from the Windows `E:` source. The later test restore exercised recovery from this snapshot.

## August 14, 2026 — application preparation and compatibility

- Recorded the original Immich paths, a 22,118-file / 38.24 GB library measurement, and roughly 526 GB total used space on `E:`.
- Created `immich-manual-backup.sql.gz`, reported at 56,789,824 bytes.
- Replaced the previous PostgreSQL image with the Immich VectorChord-compatible image; logs showed extension initialization and vector index work. A subsequent connection/import failure was tracked separately from extension compatibility.

Details: [migration preparation](documentation/migration-preparation.md) and [vector-extension incident](documentation/incident-postmortems/immich-vector-extension.md).

## Initial build period — exact dates not retained for every action

- Installed Proxmox and TrueNAS; separated compute and storage roles.
- Configured ZFS, SMB/NFS sharing, the host NFS mount, and the LXC media bind mount.
- Recovered the earlier PostgreSQL filesystem incident, corrected a wrong-subnet static address, changed short power-key handling, and increased Immich container resources.
- Resolved the multi-layer NFS access problem with a documented lab ACL tradeoff.
- Established two local Restic repositories and used `restic copy` for the second repository.

See [all incident reports](documentation/incident-postmortems/README.md). Future implemented changes should add a dated entry, update the current-state page, and link their evidence.
