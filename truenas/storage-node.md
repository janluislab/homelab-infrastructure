# Storage Node — Lenovo IdeaPad Y700

## Hardware

| Component | Specification |
|---|---|
| System | Lenovo IdeaPad Y700 15ISK |
| CPU | Intel Core i7-6700HQ |
| GPU | NVIDIA GTX 960M |
| RAM | Up to 16 GB |
| Storage | 4 TB HDD |
| Role | Dedicated NAS |

## Software

- TrueNAS Community Edition
- ZFS
- NFS
- SMB

## Responsibilities

The Y700 provides dedicated network storage for the compute node.

The storage architecture intentionally separates persistent application data from application compute.

NFS is used to expose datasets to the Proxmox environment.
