# Incident Postmortem — PostgreSQL Filesystem Error

## Severity

High

## Impact

Immich became unavailable and returned HTTP 500 errors.

PostgreSQL reported:

`could not open file "global/pg_filenode.map": Invalid argument`

## Environment

- Windows
- Docker Desktop
- WSL2
- Immich
- PostgreSQL

## Investigation

The PostgreSQL database directory was located on a Windows/WSL2-translated filesystem path.

Low-level PostgreSQL filesystem operations were failing.

The investigation focused on the storage layer rather than immediately assuming database corruption.

## Recovery

1. Shut down WSL2.
2. Updated WSL.
3. Rebooted the Windows host.
4. Restarted the PostgreSQL environment.
5. Allowed PostgreSQL crash recovery to complete.
6. Verified database availability.
7. Verified Immich functionality.

## Result

The database recovered successfully and Immich returned to normal operation.

## Root Cause

The database was operating on a Windows/WSL2 filesystem layer that was not appropriate for PostgreSQL's database workload.

## Preventive Action

Databases are now stored on native local Linux/container storage.

Network storage is reserved for appropriate file/media workloads.

## Lessons Learned

Database storage requirements differ from general file storage requirements.

Storage architecture must consider filesystem semantics, locking, durability, and database workload characteristics—not simply available capacity.
