# Incident — TrueNAS ZFS/HDD integrity investigation

[All incidents](README.md) · [Portfolio](../../README.md)

## Environment and impact

TrueNAS used the single-data-disk `tank` pool on an external 4 TB Seagate drive. ZFS reported data-error paths during the storage investigation, raising concerns about the media library and the NAS-backed application.

A single-disk layout has no redundant data disk for reconstruction, so independent backups were important to the investigation and recovery options.

## August 27, 2026 evidence

The initial status reported **3,878 data errors**. The error-path list was then reduced to **3,827 unique paths**: 3,808 media and 19 other paths. These counts describe different measures and are not interchangeable.

| Check | Recorded observation |
|---|---|
| SMART | Overall assessment passed; reallocated, pending, uncorrectable, and interface CRC counts were zero |
| Restic check | Repository check completed without reported errors |
| Separate restore | 38.244 GiB, comprising approximately 37.9 thousand files/directories |
| Sample comparison | Three flagged JPG/MP4 files had matching SHA-256 hashes against their restored backup copies |
| Later scrub/status | `tank` ONLINE; READ/WRITE/CKSUM counters zero; 0B repaired; scrub completed with zero errors; no known data errors in that captured result |

The recorded clean scrub completed in 57 minutes and 2 seconds. It described that episode's result, not a guarantee of continuing drive health.

## What the investigation established

Independent restored copies and sample hashes gave a way to check selected content without relying only on the affected pool. The later clean status was useful validation, but the retained record does not establish the original physical cause or an exhaustive validation of all migrated files.

A passing SMART assessment is not proof that a drive, enclosure, power supply, or connection is fault-free. None of those components is identified as a confirmed cause in this report.

## Later recurrence and clean recovery checkpoint

A later August 31 record included a suspended-pool observation, followed by an ONLINE pool with a scrub in progress, 5,216 data errors, and permanent-error metadata entries. This is a separate observation from the earlier clean scrub.

The later **September 28** status provided the clean storage checkpoint missing from the earlier summary:

| Check | Later recorded result |
|---|---|
| Pool | `tank` ONLINE |
| Device counters | READ/WRITE/CKSUM: 0/0/0 |
| Permanent-error section | No known data errors |
| Completed scrub shown in status | September 13, 2026; 0B repaired; zero errors; 01:14:22 |
| Restored dataset | `tank/restored`, mounted `/mnt/tank/restored`, 133G used |

The status was **read on September 28**; the scrub itself completed on September 13. The pool later reported a clean state, while the exact physical cause and intermediate repair actions remain unconfirmed.

On October 1, application/container health and user-visible functionality were also confirmed. See [application recovery](application-recovery.md) for that separate validation.

## Lessons and follow-up

Review the entire `zpool status -v` output, including permanent errors, rather than only the pool's ONLINE label. Compare suspect content against independent recovery copies and preserve final status/results after any repair.

For future incidents, retain a sanitized before/after status, the exact recovery action, a completed scrub result, and a record of which restored files were verified. Do not clear error counters and call that data repair.

Evidence: [validation record](../../evidence/validation-record.md). Reference: [OpenZFS pool status documentation](https://openzfs.github.io/openzfs-docs/man/master/8/zpool-status.8.html).
