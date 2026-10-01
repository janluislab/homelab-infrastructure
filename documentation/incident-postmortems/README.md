# Incident reports

[Back to the portfolio](../../README.md)

These reports use the same method: describe the failure, isolate the relevant layer, record the change, verify the result, and identify the remaining limitation. No incident duration, severity score, or root cause is invented where evidence is missing.

| Incident | Main diagnostic lesson | Recorded outcome |
|---|---|---|
| [Immich vector-extension compatibility](immich-vector-extension.md) | Match the application with its supported database image/extensions | Compatible image retained existing data; vector initialization progressed; later application health confirmed |
| [PostgreSQL filesystem error](postgresql-corruption.md) | Investigate the database storage path before assuming irreversible corruption | Database startup and Immich restored; later deployment uses local Linux database storage |
| [NFS permission chain](nfs-permissions.md) | Test export mapping, ownership, recursive ACLs, and effective container identity separately | Writes restored; broad ACL remains a hardening gap |
| [Proxmox network lockout](proxmox-network-lockout.md) | Compare the configured address with the actual LAN | Wrong-subnet static configuration corrected |
| [Proxmox reboot outage](proxmox-reboot-outage.md) | Ping, management services, storage, and applications require independent checks | Management access restored; exact later service-level cause not retained |
| [Unexpected shutdown](unexpected-shutdown.md) | Use the journal to distinguish a shutdown from a sleep/network issue | Power-key handling changed after a logged short press |
| [LXC resource starvation](resource-starvation.md) | Resource graphs can explain an apparently frozen console | CT100 increased to 6 GiB RAM and 2 GiB swap; services recovered |
| [Storage-format incompatibility](storage-format-incompatibility.md) | Identify the on-disk format before attempting mounts | Windows Storage Spaces disk identified; compatible source used |
| [Restore metadata warnings](restore-metadata-warnings.md) | Separate metadata portability from content recovery and skipped objects | Representative content checked; identical Windows metadata not claimed |
| [External backup-drive errors](external-backup-drive.md) | Software repair does not restore physical drive reliability | Readability recovered for the migration; device reliability remains a limitation |
| [TrueNAS ZFS/HDD investigation](truenas-storage-hdd.md) | Combine pool status, SMART, independent restore, and file comparisons | Later September 28 clean pool status and sample comparisons recorded; physical cause unconfirmed |
| [Application recovery and credential follow-up](application-recovery.md) | Recover the stack in dependency order and check the application | October 1 updater/API/application verification and Proxmox Homepage credential rotation confirmed |

Evidence comes from owner-supplied build notes, existing repository screenshots, and later terminal results reviewed for this portfolio. See the [validation record](../../evidence/validation-record.md) for dates and the exact scope of the available checks.
