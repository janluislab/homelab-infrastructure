# Recorded validation results

[Evidence index](README.md) · [Portfolio](../README.md)

This page summarizes terminal output and owner-confirmed results reviewed while completing the documentation. It is not a fabricated raw log, a new host inspection, or a substitute for a dated output capture.

## August 27, 2026 — storage investigation

| Check | Recorded result | Scope |
|---|---|---|
| SMART | Overall assessment passed; reallocated, pending, uncorrectable, and interface CRC counts were zero | Observation at the time, not proof of future drive reliability |
| Restic repository check | Completed with no errors reported | Default check; full `--read-data` coverage not demonstrated |
| Separate test restore | 38.244 GiB; 37,929 files/directories | A tested restore, not necessarily the entire migrated dataset |
| Later ZFS scrub/status | `tank` ONLINE; 0B repaired; scrub completed with zero errors; no known data errors in that captured result | Historical result for that episode |
| SHA-256 comparison | Three previously flagged JPG/MP4 files matched their Restic-restored copies | Three sampled files, not all media |

The initial error list contained 3,878 data errors; deduplication identified 3,827 unique paths, including 3,808 media and 19 other paths. Error counts and unique-path counts measure different things.

## Later storage recurrence

An August 31 record included a suspended-pool observation followed by an ONLINE pool with a scrub in progress, 5,216 data errors, and metadata entries in the permanent-error section. The reviewed record did not include the final storage repair sequence or an established hardware cause for that recurrence.

Keep this separate from the clean August 27 result. Subsequent application recovery is not equivalent to a new clean full-pool scrub.

## October 1, 2026 — Immich service verification

Recorded command:

```bash
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml ps
```

| Container | Recorded state |
|---|---|
| `immich_server` | Running and healthy; port 2283 published |
| `immich_machine_learning` | Running and healthy |
| `immich_postgres` | Running and healthy |
| `immich_redis` | Running |

The owner confirmed that everything was working after this check. The update workflow had pulled release images, recreated the relevant services, and reported application health. No exact image digest or measured recovery duration is inferred from that summary.

## Credential and dashboard follow-up

An exposed application/update secret was regenerated. The recorded Proxmox command restarted Homepage in CT102, and the owner confirmed the dashboard result was good. The secret, any earlier secret-bearing configuration, and private service URLs are excluded.

See [application recovery](../documentation/incident-postmortems/application-recovery.md), [storage investigation](../documentation/incident-postmortems/truenas-storage-hdd.md), and [security limitations](../security/controls-and-limitations.md).
