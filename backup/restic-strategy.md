# Restic backup and recovery strategy

[Back to the portfolio](../README.md)

## Implemented during the migration

The source data was protected with two encrypted, deduplicated Restic repositories on separate local storage devices. Preparation recorded roughly 526 GB total source-drive usage, a 38.24 GB Immich library inventory, and a separate 38.244 GiB test restore. These are different measurement scopes; see [migration preparation](../documentation/migration-preparation.md).

| Copy | Role | Recorded validation |
|---|---|---|
| Source data | Original working data during migration | File counts, size, and application/file checks |
| Restic repository 1 | First independent local backup | `restic check`; test restore |
| Restic repository 2 | Additional repository on a separate local device | Snapshots copied with `restic copy`; repository checks reported in build notes |

The second repository was populated with Restic's repository-copy operation rather than rereading the source. Both repositories are local. The project **does not yet claim a completed 3-2-1/offsite backup arrangement**. Backblaze B2 was evaluated as a future offsite destination.

## What the checks establish

A normal `restic check` validates repository structure and consistency. It does **not** reread every stored data pack. A full data read requires `restic check --read-data`; a subset check gives narrower coverage.

The build record confirms checks and restores, but does not preserve evidence of a full `--read-data` pass over both repositories. The later storage investigation recorded a successful repository check, a 38.244 GiB test restore comprising approximately 37.9 thousand files/directories, and three sample files whose SHA-256 hashes matched the restored backup copies.

Those are useful recovery observations. They are not a claim that every source file was exhaustively compared or that either backup device is immune to failure.

## Verification examples

Use private repository paths and an existing protected password file. The following commands are future runbook examples, not a transcript of a newly completed backup job:

```bash
export RESTIC_REPOSITORY=/path/to/repository
export RESTIC_PASSWORD_FILE=/path/to/private-password-file
restic snapshots
restic check
restic check --read-data
```

A full data check can take substantial time. To test restoration without overwriting working files:

```bash
restic restore SNAPSHOT_ID --target /path/to/separate-test-restore
```

Choose a specific snapshot, compare selected restored files with their originals, open representative photos/videos, and retain a dated summary of the results. The restore target must be separate from the live library.

## Application recovery requirements

Immich has two separate recovery concerns: media on TrueNAS and PostgreSQL on local container storage. A copy of the media library alone cannot restore the full application state.

A manual logical PostgreSQL backup was produced during the original preparation and recorded at 56,789,824 bytes. This backup-file record is separate from a demonstrated restore of that exact SQL dump. See [database preparation](../documentation/migration-preparation.md).

For future scheduled protection, pair media backups with a supported, application-consistent PostgreSQL backup and the required private application configuration. Do not assume that copying a running database directory produces a consistent database backup. Automated schedules, retention, recovery-time targets, and offsite delivery are not demonstrated by this repository yet.

## External-drive incident

An external drive encountered errors during backup copying, and `chkdsk` reported bad sectors/cluster remapping. The drive became readable enough to continue the recovery work and was checked again.

That did not repair its physical media or establish it as a reliable long-term backup target. See the [external-drive incident](../documentation/incident-postmortems/external-backup-drive.md).

## Recovery order

1. Verify the selected repository and snapshot before changing working data.
2. Restore into a separate destination and inspect representative files.
3. Restore the database using its supported recovery procedure.
4. Verify NFS/bind mounts and application storage settings.
5. Start services, check logs/health, and test the library in Immich.
6. Record the snapshot used, validation scope, and any remaining uncertainty.

References: [Restic repository checks and copying](https://restic.readthedocs.io/en/stable/045_working_with_repos.html), [Restic restoration](https://restic.readthedocs.io/en/stable/050_restore.html), and [Immich database backup guidance](https://docs.immich.app/administration/backup-and-restore/).
