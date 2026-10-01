# Migration preparation and recovery copies

[Portfolio](../README.md) · [Migration overview](project-overview.md) · [Backup strategy](../backup/restic-strategy.md)

## Original environment

Immich ran on Windows/Docker with files under `E:\Homelab\Immich_Server`. Nextcloud and other Docker workloads also existed in the earlier environment; the documented new-platform migration centers on Immich.

| Original directory | Container use |
|---|---|
| `Library` | `/usr/src/app/upload` |
| `Database` | `/var/lib/postgresql/data` |
| `ML_Cache` | `/cache` |

These are historical Windows deployment mappings. The new platform keeps PostgreSQL on local Linux/container storage and supplies media over TrueNAS/NFS.

## Inventory before migration

The preparation separated application data, database state, recovery copies, and disk-format constraints before relying on a new host.

| Measurement | Date/record | Meaning |
|---|---|---|
| Roughly 526 GB used on `E:` | August 14 drive inventory | Total source-drive usage, not the size of the Immich library |
| 22,118 files / 38.24 GB | August 14 Windows library inventory | Measured Immich library at that checkpoint |
| 56,789,824-byte `.sql.gz` file | August 14 manual backup record | Size of the produced database backup file |
| 38.244 GiB restored in 24:51 | August 27 test-restore output | The separately tested restore operation |
| 133G used by `tank/restored` | September 28 `zfs list` | Later dataset usage; includes a different scope and storage accounting |

These figures are different measurements. They should not be added together or described as identical copies of one dataset. The older project summary used approximately 525 GB for the migration scope; the directly retained measurements above are the clearer evidence for the public portfolio.

## PostgreSQL backup preparation

The original environment contained dated automatic Immich database-backup files. A separate manual logical backup was also produced as `Database\immich-manual-backup.sql.gz`.

Historical command recorded in the Windows session:

```text
docker exec immich_postgres sh -c "pg_dumpall -U postgres | gzip" > Database\immich-manual-backup.sql.gz
```

The recorded result was a 56,789,824-byte file. This establishes that a backup file was produced; a separate restore of this specific SQL dump is not demonstrated by its file-size record. The command is history, not a recommendation to use identical native-output redirection in every Windows shell.

A logical database backup protects a different part of the application than the media library. Database dump contents and credentials are excluded from GitHub.

## Restic snapshot and independent test restore

The retained snapshot was dated August 20, 2026 and originated from the `E:` source. `restic check` later reported no errors.

A test restore into `C:\Immich-Restic-Test` completed in 24 minutes and 51 seconds and restored 38.244 GiB. The progress/result capture reported roughly 37,929 files/directories; this mixed object count is not the same as the earlier count of regular library files.

The separate destination allowed representative files and hashes to be checked without overwriting the working source. The later storage investigation compared three suspect JPG/MP4 files against copies recovered from Restic.

## Migration decisions

- Keep source/recovery copies available while validating the destination.
- Identify the source disk format before mounting; a Windows Storage Spaces disk required a compatible alternate transfer path.
- Check both database/image compatibility and file-storage integration.
- Validate NFS access on the Proxmox host before checking the LXC/Docker layers.
- Verify application behavior after the storage and container changes.

Details: [vector compatibility](incident-postmortems/immich-vector-extension.md), [storage-format incident](incident-postmortems/storage-format-incompatibility.md), [NFS incident](incident-postmortems/nfs-permissions.md), and [validation record](../evidence/validation-record.md).
