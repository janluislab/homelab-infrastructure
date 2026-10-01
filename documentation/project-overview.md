# Project overview and migration record

[Back to the portfolio](../README.md)

## Objective

Build a maintainable infrastructure lab from existing hardware and use real workloads to practice Linux administration, virtualization, storage, backup, incident response, and Security+ concepts.

The project replaced the previous desktop-based hosting arrangement with a dedicated Proxmox compute node and TrueNAS storage node. Approximately **525 GB** of photos, documents, and application data were backed up, migrated, and checked during the build.

## Hardware and roles

| Role | Hardware | Recorded specification |
|---|---|---|
| Compute | Lenovo Legion Y540 15IRH | Intel i7-9750H; NVIDIA RTX 2060; build notes list up to 16 GB RAM |
| Storage | Lenovo IdeaPad Y700 15ISK | Intel i7-6700HQ; NVIDIA GTX 960M; build notes list up to 16 GB RAM; external 4 TB HDD |

The GPUs are hardware inventory, not evidence of deployed GPU passthrough. IOMMU was enabled during the build for future experimentation.

## Migration stages

| Stage | Work performed | Validation or decision |
|---|---|---|
| Preserve the source | Created encrypted Restic repositories before retiring the earlier setup | Repository checks and a second repository copy |
| Recover the application | Investigated the PostgreSQL filesystem error on Windows/WSL2 | Database startup and Immich functionality restored after host/WSL recovery |
| Build the hosts | Installed Proxmox and TrueNAS; created the ZFS pool | Management access and host/storage configuration checked |
| Restore data | Restored the existing data to TrueNAS | File counts, total size, sample files, and later recovery comparisons |
| Integrate NFS | Mounted the export on Proxmox and bound it into CT100 | Host and container permission tests isolated multiple blockers |
| Deploy Immich | Used Docker Compose in LXC; kept PostgreSQL on local storage | Increased CT100 resources; services returned healthy |
| Document and extend | Added a Cloudflare container and later an operations container | Proxmox screenshots; later Homepage and application verification |

These are the reported build stages, not a claim that every migrated file received a full hash comparison. The later ZFS investigation includes three explicitly recorded file-hash comparisons and a test restore.

## Engineering lessons

- A database needs suitable filesystem behavior, not just enough capacity.
- Successful host access to NFS does not prove that an unprivileged container can write to it.
- Network reachability, the management UI, mounted storage, and application health are separate checks.
- Repository checks and test restores make backup quality measurable.
- A clean scrub and SMART result describe observations at that time; neither guarantees future hardware health.
- An ACL workaround can restore service while leaving security work to complete.

The [incident reports](incident-postmortems/README.md) preserve those decisions and identify uncertainty where retained evidence does not establish an exact root cause.

## Recorded status and roadmap

| Component | Status supported by the project record |
|---|---|
| Proxmox, TrueNAS/ZFS, NFS, Immich, Docker Compose | Deployed |
| Cloudflare Tunnel in CT101 | Deployed; resource configuration captured in a screenshot |
| Homepage and update tooling in CT102 | Later deployment reported; Homepage restart and credential rotation confirmed |
| Immich services | Server, PostgreSQL, and machine learning healthy; Redis running in October 1, 2026 Compose output |
| Two local Restic repositories | Implemented; checks, repository copy, and restores recorded |
| Nextcloud, VLANs, OPNsense, WireGuard | Planned |
| Centralized logging/SIEM and vulnerability scanning | Planned |
| Backblaze B2 or another offsite backup destination | Planned; local copies do not complete 3-2-1 protection |

Documentation reviewed on **October 1, 2026** against build notes, checked-in screenshots, and later troubleshooting records. This update did not connect to or reconfigure the running homelab.
