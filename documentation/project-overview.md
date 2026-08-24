# Two-Node Proxmox + TrueNAS Infrastructure Lab

## Overview

A production-style homelab infrastructure project built from two aging Lenovo laptops.

The objective is to design, deploy, migrate, secure, and document a small infrastructure environment while applying practical Linux administration, virtualization, storage, networking, backup, disaster recovery, and cybersecurity concepts.

## Project Objectives

- Build a dedicated compute node using Proxmox VE.
- Build a dedicated storage node using TrueNAS and ZFS.
- Separate application compute from persistent storage.
- Migrate approximately 525 GB of existing production data.
- Deploy Immich using LXC and Docker Compose.
- Implement NFS-based storage between the nodes.
- Keep application databases on local storage.
- Implement and verify a 3-2-1 backup strategy.
- Document real infrastructure incidents and their root causes.
- Apply practical Security+ concepts.
- Expand the environment with VLANs, OPNsense, WireGuard, centralized logging, and vulnerability scanning.

## Current Status

The infrastructure foundation is operational.

Completed:

- Proxmox VE deployment
- TrueNAS deployment
- ZFS storage
- NFS storage integration
- Immich migration
- PostgreSQL recovery
- Restic backup implementation
- Backup integrity verification
- NFS permission troubleshooting
- LXC resource tuning

Planned:

- Nextcloud
- VLAN segmentation
- OPNsense
- WireGuard
- Centralized logging/SIEM
- Vulnerability scanning
- Backblaze B2 offsite backup
