# Incident record — Application recovery and credential follow-up

[All incidents](README.md) · [Portfolio](../../README.md)

## Context

The troubleshooting record included recovery of Immich after host/storage interruptions and a stopped Compose stack. The workload ran in CT100, with media on TrueNAS/NFS and the database on local container storage. Cloudflare operated separately in CT101; later operations tooling and Homepage ran in CT102.

This report records the verified application outcome. It does not assign one unproven cause to every preceding outage.

## Recorded recovery and verification

On September 30, the enabled/running `homelab-immich-updater` service was recorded in CT102 using Python and `/opt/homelab/ops/app.py`. The restricted SSH update path ran successfully, and locking logic was present in `/usr/local/sbin/immich-update`.

On October 1, 2026, that manual workflow pulled release images, recreated relevant services, waited for the API, and completed with application health confirmed. See the [operations-node implementation record](../../proxmox/operations-node.md). The subsequent validation command was:

```bash
pct exec 100 -- docker compose -f /opt/immich/docker-compose.yml ps
```

The recorded states were:

| Container | State |
|---|---|
| `immich_server` | Running and healthy; port 2283 published |
| `immich_machine_learning` | Running and healthy |
| `immich_postgres` | Running and healthy |
| `immich_redis` | Running |

The owner confirmed afterward that everything was working. Redis is described as running because the supplied output did not label it healthy.

## Credential follow-up

The **Proxmox API credential used by Homepage** was regenerated, followed by a recorded restart of Homepage in CT102 and confirmation that its Proxmox integration worked.

This is the confirmed rotation. Completion of the other service-key rotations was not recorded. Credential values and private configuration remain excluded.

## Boundaries and lesson

The later September 28 storage output separately recorded a clean pool state. An external Cloudflare route test and the precise storage-repair sequence are not retained. Container health, user-visible functionality, remote-route health, and pool integrity are tracked as separate observations.

Recover dependencies first, verify the local application, then verify access/dashboard integrations. Use a dated result for each layer instead of relying on a single successful page load.

See the [operations runbook](../operations-runbook.md), [validation record](../../evidence/validation-record.md), and [security controls](../../security/controls-and-limitations.md).
