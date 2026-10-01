# Operations node — CT102

[Portfolio](../README.md) · [Current deployment](../documentation/current-state.md) · [Change history](../CHANGELOG.md)

## Purpose

CT102 separates dashboard and operational tooling from Immich's application container. Homepage provides the front door; a Python web service runs the manually triggered Immich updater.

The September 30 terminal output showed `homelab-immich-updater` active/running and enabled, using `/usr/bin/python3 /opt/homelab/ops/app.py`. It listened on the operations node's home-LAN address at port 8081.

## Recorded components

| Component | Location or role |
|---|---|
| Homepage | Dashboard container in CT102 |
| Operations service | `homelab-immich-updater`, managed by systemd in CT102 |
| Python program | `/opt/homelab/ops/app.py` |
| Controlled update command | `/usr/local/sbin/immich-update` on Proxmox `pve`; invokes `pct exec 100` |
| Target workload | CT100 Compose stack at `/opt/immich/docker-compose.yml` |
| Application check | Immich API health after pull/recreation |

The implementation's private keys, credential-bearing files, and exact private access URL are excluded. This page documents the installed workflow; it does not reconstruct a source-code release from incomplete snippets.

## Update flow and verification

| Step | Operation | Recorded validation |
|---|---|---|
| Trigger | Homepage → Operations Center → Immich Updater | Manually triggered workflow used |
| Delegate | CT102 connects by restricted SSH to Proxmox `pve`; forced command invokes the CT100 update | Successful restricted SSH test recorded |
| Serialize | Update script uses locking | Locking lines present in the installed script |
| Update | Pull the deployment images and recreate the Compose services | Successful run log on October 1 |
| Validate | Wait for the application API to become healthy | API health success logged; Compose states checked |
| Confirm | Open/use the application | Owner confirmed everything was working |

The restricted SSH test produced a PTY/noninteractive warning and still executed the intended update successfully. The absence of an interactive terminal is consistent with the configured execution path; the update's result, rather than that warning alone, was used to assess success.

Execution ownership is **CT102 web interface → restricted SSH to `pve` → forced `/usr/local/sbin/immich-update` command → `pct exec 100` → Immich Compose**. The Python operations program runs in CT102; the forced update command runs on the Proxmox host.

## Access and change boundaries

Forced-command/no-forwarding SSH restrictions were installed. The permitted command path was exercised; separate rejection tests for arbitrary commands and a simultaneous-update/lock-contention test were not recorded.

The updater is intended for the trusted home LAN and is not to be exposed through Cloudflare. The recorded private-address listener supports that deployment intent; a complete firewall/external-exposure test is not claimed.

Updates are manually initiated. The PostgreSQL/VectorChord image remains the recorded compatibility baseline; automatic database major-version upgrades are not part of the demonstrated workflow. A successful update does not imply that an automatic pre-update database backup or automated rollback was implemented.

## Read-only operational checks

Run on the Proxmox host for a future check:

```bash
pct exec 102 -- systemctl is-active homelab-immich-updater
pct exec 102 -- systemctl is-enabled homelab-immich-updater
pct exec 102 -- journalctl -u homelab-immich-updater --no-pager -n 60
pct exec 102 -- docker ps --filter name=homepage
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml ps
```

These examples inspect deployment state; they are not new executions during the documentation update. Review logs before publishing them.

The Proxmox API token used by Homepage was regenerated on October 1, followed by a Homepage restart and successful widget confirmation. See [security controls](../security/controls-and-limitations.md) and [application recovery](../documentation/incident-postmortems/application-recovery.md).
