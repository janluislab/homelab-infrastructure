# Portfolio guide for employers

[Portfolio](../README.md) · [Current deployment](current-state.md) · [Change history](../CHANGELOG.md)

## Project in context

I built and operated a personal infrastructure lab using two repurposed Lenovo laptops. The project moves an existing self-hosted photo application from Windows/Docker to a Proxmox compute node and TrueNAS storage node, then extends it with operational tooling.

My responsibilities included preparing recovery copies, configuring the hosts and storage integration, resolving application and infrastructure failures, testing the recovered environment, and documenting the decisions and remaining risks.

## Read the project by engineering question

| Question | Where to look | What the record demonstrates |
|---|---|---|
| What is deployed now? | [Current deployment](current-state.md) | Service roles, software/image records, data paths, and dated verification |
| How was existing data protected? | [Migration preparation](migration-preparation.md) and [backup strategy](../backup/restic-strategy.md) | Inventory, logical database backup, encrypted repository checks, and a separate test restore |
| Why is the system designed this way? | [Architecture](../architecture/infrastructure-diagram.md) | Compute/storage separation, local database placement, mount dependencies, and tradeoffs |
| How were permissions troubleshot? | [NFS case study](incident-postmortems/nfs-permissions.md) | Layer-by-layer tests across export mapping, ownership, ACL recursion, and LXC identities |
| How was storage integrity investigated? | [ZFS case study](incident-postmortems/truenas-storage-hdd.md) | SMART, pool status, independent restoration, sample hashes, and later clean status |
| How were application changes controlled? | [Operations node](../proxmox/operations-node.md) | A Python/systemd dashboard service, constrained remote execution, update locking, and API verification |
| Can someone else understand the recovery process? | [Operations runbook](operations-runbook.md) | Network → storage → mount → application → access checks, with explicit success criteria |
| What still needs work? | [Security record](../security/controls-and-limitations.md) and [roadmap](project-overview.md) | Clear separation of implemented controls, accepted lab constraints, and planned improvements |

## Skills demonstrated

| Skill | Concrete example |
|---|---|
| Linux administration | Inspecting journals, systemd service state, network configuration, filesystem mounts, and process/resource behavior |
| Virtualization and containers | Proxmox LXC allocation, unprivileged UID mapping, Docker Compose deployment, and workload separation |
| Storage administration | ZFS status/scrubs, NFS/SMB integration, dataset ACLs, and on-disk format diagnosis |
| Backup and recovery | PostgreSQL logical backup, Restic checks/copying, a separate restore, and content comparison |
| Application troubleshooting | Distinguishing filesystem errors, vector-extension compatibility, connection failures, and resource starvation |
| Operations engineering | Connecting Homepage to a controlled updater and verifying the application after an update |
| Security judgment | Explaining excessive-permission tradeoffs, protecting credentials, constraining updater access, and identifying missing offsite protection |
| Technical communication | Dated change records, configuration/evidence links, reproducible runbook checks, and incident conclusions matched to the available evidence |

## Evidence and scope

The repository includes actual Proxmox screenshots and dated summaries of terminal results from the build and recovery work. It records the difference between an observed outcome, a configuration present in the source, and a separately exercised failure test.

For example, the updater's restricted SSH path was tested successfully, and locking was present in the script. A separate concurrent-run/lock-contention test is not recorded. The ZFS pool later reported no known data errors, while the physical cause of the earlier incident remained unconfirmed.

This is a working personal lab with production-style operational practices. It is not presented as a high-availability enterprise platform, an audited security deployment, or a completed certification.
