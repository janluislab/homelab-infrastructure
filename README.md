# Two-Node Infrastructure & Security Lab

> A production-style homelab built from two aging Lenovo laptops to demonstrate practical Linux administration, virtualization, storage, networking, backup/disaster recovery, and cybersecurity skills.

![Status](https://img.shields.io/badge/Status-Active-success)
![Proxmox](https://img.shields.io/badge/Proxmox-VE%209.2-orange)
![TrueNAS](https://img.shields.io/badge/TrueNAS-25.10-blue)
![ZFS](https://img.shields.io/badge/Storage-ZFS-blueviolet)
![Linux](https://img.shields.io/badge/Linux-Debian%2013-red)
![Security%2B](https://img.shields.io/badge/CompTIA-Security%2B-yellow)

---

## 🎯 Project Overview

This project is a hands-on infrastructure and security lab designed to simulate a small production environment using two repurposed laptops.

The architecture intentionally separates **compute** from **storage** to reduce the blast radius of failures and demonstrate defense-in-depth principles.

The environment currently hosts self-managed applications including **Immich**, with **Nextcloud, network segmentation, secure remote access, centralized logging, and vulnerability scanning** planned as the lab evolves.

The project also documents real infrastructure incidents encountered during migration, including database filesystem corruption, NFS permission failures, network misconfiguration, unexpected shutdowns, resource exhaustion, and storage-format incompatibilities.

---

# 🏗️ Architecture

```text
                         INTERNET
                            │
                            ▼
                       HOME ROUTER
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
        ┌─────────────────┐   ┌─────────────────┐
        │   LENOVO Y540   │   │   LENOVO Y700   │
        │   COMPUTE NODE  │   │  STORAGE NODE   │
        │                 │   │                 │
        │  Proxmox VE 9.2 │   │ TrueNAS 25.10   │
        │                 │   │                 │
        │  LXC Containers │   │      ZFS        │
        │        │        │   │       │         │
        │   ┌────┴────┐   │   │       │         │
        │   │ Immich  │   │   │      4TB        │
        │   │Nextcloud│   │   │      HDD        │
        │   └────┬────┘   │   │       │         │
        │        │        │   │       │         │
        │ Local DB Storage│◄──┤       │         │
        │                 │ NFS               │
        └─────────────────┘   └─────────────────┘
