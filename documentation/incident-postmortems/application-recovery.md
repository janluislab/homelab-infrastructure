# Incident record — Application recovery and credential follow-up

[All incidents](README.md) · [Portfolio](../../README.md)

## Context

The troubleshooting record included recovery of Immich after host/storage interruptions and a stopped Compose stack. The workload ran in CT100, with media on TrueNAS/NFS and the database on local container storage. Cloudflare operated separately in CT101; later operations tooling and Homepage ran in CT102.

This report records the verified application outcome. It does not assign one unproven cause to every preceding outage.

## Recorded recovery and verification

On October 1, 2026, the existing update workflow pulled release images, recreated relevant services, and reported application health. The subsequent validation command was:

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

An earlier application/update secret had been disclosed during configuration troubleshooting. The secret was regenerated, followed by a recorded restart of Homepage in CT102 and confirmation that the result was good.

The portfolio excludes the secret value and earlier secret-bearing configuration. It does not claim that all credentials across the lab were rotated.

## Boundaries and lesson

No final tunnel route test or complete storage repair transcript accompanies the final application output, so neither is invented here. Container health, user-visible functionality, remote-route health, and full-pool integrity are separate observations.

Recover dependencies first, verify the local application, then verify access/dashboard integrations. Use a dated result for each layer instead of relying on a single successful page load.

See the [operations runbook](../operations-runbook.md), [validation record](../../evidence/validation-record.md), and [security controls](../../security/controls-and-limitations.md).
