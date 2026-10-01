# Two-Node Infrastructure & Security Lab

**Janluis Torres · Linux administration, virtualization, storage, and recovery**

I repurposed two Lenovo laptops into a Proxmox compute host and a dedicated TrueNAS storage server, migrated an existing Immich deployment, and built a dashboard-operated update workflow. This portfolio explains the preparation, design decisions, failures, fixes, and verification behind the working environment.

**Start here:** [Guide for employers](documentation/for-employers.md) · [Current deployment](documentation/current-state.md) · [Dated change history](CHANGELOG.md)

## Architecture

![Architecture: local application databases on Proxmox, media on TrueNAS over NFS, and independent Restic repositories](architecture/infrastructure-diagram.svg)

**Storage path:** TrueNAS ZFS dataset → NFS mount on Proxmox → bind mount in the Immich container. **PostgreSQL stays on local Linux/container storage.**

[Architecture and tradeoffs](architecture/infrastructure-diagram.md) · [Compute node](proxmox/compute-node.md) · [Storage node](truenas/storage-node.md) · [Operations node](proxmox/operations-node.md)

## Work and results

| Area | Work performed | Recorded result |
|---|---|---|
| Virtualization | Proxmox with separate Immich, Cloudflare, and operations LXC workloads | CT100/CT101 configuration captured; later CT102 service deployment confirmed |
| Migration preparation | Inventory, PostgreSQL backup, Restic snapshot/checks, and independent test restore | Source library: 22,118 files / 38.24 GB; test restore: 38.244 GiB |
| Storage integration | TrueNAS/ZFS, NFS mount, LXC bind mount, and permission troubleshooting | NAS-backed media accessible; database remains on local Linux storage |
| Incident recovery | Investigated filesystem, vector-extension, network, power, resource, and ZFS errors | [12 case studies](documentation/incident-postmortems/README.md) with evidence and bounded conclusions |
| Operations | Homepage and a Python/systemd Immich updater using restricted SSH | Pull → recreate → API health verification completed successfully on October 1, 2026 |
| Credential maintenance | Regenerated the Proxmox API token used by Homepage | Dashboard integration checked after container restart |

The source drive reported roughly **526 GB used**, while the measured Immich library and test restore were separate, smaller datasets. [Preparation and measurement details](documentation/migration-preparation.md) explain the scope of each figure.

## Latest verification

- **Storage, September 28:** `tank` ONLINE; READ/WRITE/CKSUM counters zero; no known data errors. The displayed completed scrub was dated September 13 and reported zero errors.
- **Application, October 1:** Immich server, PostgreSQL, and machine learning healthy; Redis running. Application functionality confirmed afterward.
- **Operations, September 30–October 1:** updater service active/enabled; successful controlled update; Proxmox credential regenerated and Homepage integration confirmed.

[Detailed validation record](evidence/validation-record.md) · [Screenshots](evidence/README.md) · [Recovery runbook](documentation/operations-runbook.md)

## Selected case studies

- [NFS permissions across TrueNAS, Proxmox, and unprivileged LXC](documentation/incident-postmortems/nfs-permissions.md)
- [ZFS/HDD investigation with later clean storage validation](documentation/incident-postmortems/truenas-storage-hdd.md)
- [Immich vector-extension and PostgreSQL image compatibility](documentation/incident-postmortems/immich-vector-extension.md)
- [PostgreSQL filesystem incident during migration](documentation/incident-postmortems/postgresql-corruption.md)

[All incident reports](documentation/incident-postmortems/README.md) · [Backup and recovery](backup/restic-strategy.md)

## Security and roadmap

This personal lab applies concepts studied for **CompTIA Security+**. Implemented controls and their tradeoffs are documented: encrypted backups, container identity mapping, restricted updater execution, service separation, and credential rotation.

The pool has one data disk; the hosts do not provide high availability. A broad dataset ACL used during recovery remains a hardening item, and the backup repositories are local. [Security controls and limitations](security/controls-and-limitations.md)

Nextcloud migration to the new platform, VLAN segmentation, OPNsense, WireGuard, centralized logging/SIEM, vulnerability scanning, and offsite backup are **planned**. Nextcloud existed in the earlier Windows environment; deployment on the new platform is not claimed.

[Project history and roadmap](documentation/project-overview.md) · [How documentation stays current](documentation/documentation-workflow.md)
