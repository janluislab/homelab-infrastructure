# Incident — PostgreSQL filesystem error during migration

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and evidence

Immich returned HTTP 500 errors in the earlier Windows/Docker Desktop/WSL2 environment. PostgreSQL reported:

```text
could not open file "global/pg_filenode.map": Invalid argument
```

The database directory was on a path translated through the Windows/WSL2 filesystem layer. This focused the investigation on storage behavior and the host/VM state, rather than immediately discarding the database.

## Investigation and recovery

The recorded sequence was to shut down WSL2, update WSL, reboot the Windows host, and restart the application environment. PostgreSQL completed its startup/crash-recovery process, after which database availability and Immich functionality were verified.

The recovery reused the database. No manual deletion of database files or forced WAL reset is claimed.

## Cause and confidence

The working diagnosis was an interaction between the database workload and its Windows-translated storage path/host state. Recovery after the WSL/host reset supports that diagnosis. The retained evidence does not establish a particular Docker or PostgreSQL software defect, nor prove that permanent on-disk database corruption occurred.

The filename is retained from the original repository, but the observed failure is described as a **filesystem incident** rather than a conclusively proven database-corruption event.

## Prevention and lesson

The migrated deployment keeps PostgreSQL on native local Linux/container storage. TrueNAS/NFS supplies media files separately.

Database recovery also needs an application-consistent database backup; a media-library copy alone is insufficient. See the [backup record](../../backup/restic-strategy.md).

Reference: [Immich system requirements and database storage guidance](https://docs.immich.app/install/requirements/).
