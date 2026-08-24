# Compute Node — Lenovo Legion Y540

## Hardware

| Component | Specification |
|---|---|
| System | Lenovo Legion Y540 15IRH |
| CPU | Intel Core i7-9750H |
| GPU | NVIDIA RTX 2060 |
| RAM | Up to 16 GB |
| Role | Compute / Virtualization |

## Software

- Proxmox VE 9.2
- Debian 13-based
- LXC containers
- Docker Compose
- Immich
- Nextcloud (planned)

## Responsibilities

The Y540 acts as the compute node for the homelab.

Applications run inside isolated LXC containers while persistent application databases remain on local container storage.

Large media and file datasets are provided by the dedicated TrueNAS storage node over NFS.
