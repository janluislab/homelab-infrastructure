# Compute node — Lenovo Legion Y540

[Back to the portfolio](../README.md)

The Y540 hosts Proxmox, LXC containers, and application compute. Checked-in screenshots display **Proxmox VE 9.2.10**; they are an August 2026 configuration snapshot, not a live version check.

## Hardware

| Component | Recorded specification |
|---|---|
| System | Lenovo Legion Y540 15IRH |
| CPU | Intel Core i7-9750H |
| GPU | NVIDIA RTX 2060 |
| RAM | Build notes list up to 16 GB; exact installed capacity not captured here |
| Role | Virtualization and application hosting |

## Containers

| ID | Workload | Recorded allocation | Evidence |
|---|---|---|---|
| 100 | Immich with Docker Compose, PostgreSQL, Redis, and machine learning | 1 CPU core; 6 GiB RAM; 2 GiB swap; 28 GB local root disk | [CT100 resources](../evidence/proxmox/lxc-immich-resources.png) |
| 101 | Cloudflare Tunnel | 1 CPU core; 512 MiB RAM; 512 MiB swap; 8 GB local root disk | [CT101 resources](../evidence/proxmox/llxc-coudflare-resources.png) |
| 102 | Operations/Homepage and update tooling | Allocation not captured in the checked-in screenshots | Later troubleshooting record |

The first two allocations come directly from the screenshots. CT102 was added later, so it does not appear in the older dashboard image.

## Immich storage

The CT100 screenshot records this mount-point mapping:

```text
Host:      /mnt/pve/immich-nfs
Container: /mnt/immich-data
```

TrueNAS supplies the media directory over NFS. PostgreSQL remains on local Linux/container storage. The screenshot alone does not verify every Docker volume; database placement comes from the build record.

Proxmox's storage screenshot lists `immich-nfs` and `local`. The storage object's available content types do not prove that all those types are in use. This project uses NFS for the documented media bind mount.

## Resource and availability decisions

Immich initially ran in a 512 MB container and became unresponsive under memory and swap pressure. Increasing the allocation to 6 GiB RAM and 2 GiB swap restored service. That allocation is a documented result for this workload, not a universal sizing recommendation.

The laptop also required investigation of an unexpected shutdown. A `systemd-logind` power-key event explained the outage, and power-key handling was changed after the cause was identified.

See [resource starvation](../documentation/incident-postmortems/resource-starvation.md), [unexpected shutdown](../documentation/incident-postmortems/unexpected-shutdown.md), and [network/reboot recovery](../documentation/incident-postmortems/proxmox-network-lockout.md).

## Read-only checks for future validation

Run on the Proxmox host:

```bash
pveversion
pct list
pct config 100
findmnt -T /mnt/pve/immich-nfs
systemctl is-active pveproxy pvedaemon
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml ps
```

These are runbook examples; they were not executed against the live host during this documentation update. Keep full configuration captures private until credentials and identifying details have been reviewed.
