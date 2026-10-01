# Security controls and limitations

[Back to the portfolio](../README.md)

This lab applies concepts studied for CompTIA Security+. It records implemented decisions and the gaps that remain; it does not claim an audited production environment or a completed certification.

## Implemented decisions

| Decision | Security purpose | Boundary of the claim |
|---|---|---|
| Separate compute and storage hosts | Independent administration and clearer failure boundaries | Both still share the home LAN; this is not network segmentation |
| Unprivileged LXC identity mapping used during deployment | Container root maps to a distinct host identity | Export mappings and dataset ACLs still determine effective storage access |
| Keep PostgreSQL off NFS and Windows-translated paths | Match storage behavior to database durability needs | Local storage still needs an application-consistent backup |
| Encrypt Restic repositories | Protect backup contents when repository storage is accessed | Password/key protection and independent recovery copies remain necessary |
| Separate Cloudflare Tunnel container | Keep tunnel operation separate from the Immich workload | A tunnel alone does not establish authentication policy or least privilege |
| Constrain updater SSH execution | Forced-command/no-forwarding configuration limits the intended command path | Permitted update path tested; arbitrary-command rejection tests not recorded |
| Serialize updater execution | Locking logic present in the update script | Separate concurrent-run/lock-contention test not recorded |
| Keep operations listener on the home LAN | Python service recorded on a private LAN address at port 8081 | Full firewall/external-exposure audit not recorded; keep it off Cloudflare |
| Rotate the Proxmox API credential used by Homepage | Replace the credential and check the dashboard integration | This specific rotation was confirmed; all-service rotation not claimed |
| Exclude private files from version control | Reduce accidental publication of secrets and application data | `.gitignore` is a safeguard, not a secret scanner or access-control mechanism |

## Known access-control tradeoff

The NFS recovery used Maproot identity settings and a broad Everyone/Full Control dataset ACL after UID/GID and recursive permission issues blocked access. The build treated this as a private, subnet-restricted lab workaround.

This is **not least privilege**. A restricted subnet does not prevent an authorized client or compromised lab host from abusing excessive file permissions. The exact current export configuration is not captured in the screenshots.

The hardening target is a dedicated media identity with explicit UID/GID mapping, narrow client authorization, and only the required dataset permissions. Any change should first be tested against both existing media access and new uploads. This target is planned, not claimed as completed.

## Availability and recovery constraints

- The ZFS pool has one data disk and no redundant data device.
- Independent compute and storage hosts do not provide high availability.
- Local backup copies remain exposed to a shared physical-site incident.
- Media copies do not replace a database backup.
- No implemented SIEM, vulnerability-scanning program, or VLAN/firewall design is demonstrated here.

## Public documentation hygiene

Store passwords, API keys, Cloudflare credentials, private SSH keys, Restic password files, and real `.env` files outside this repository. Publish sanitized configuration examples only. Review screenshots and terminal captures before adding them to the evidence directory.

The Proxmox API credential used by Homepage was regenerated and Homepage restarted; its Proxmox integration was confirmed working. Completion of the remaining service-key rotations was not recorded. Secret values and private configuration are excluded.

See the [operations-node record](../proxmox/operations-node.md) for the updater controls and the tests actually performed.

Related: [NFS incident](../documentation/incident-postmortems/nfs-permissions.md), [backup strategy](../backup/restic-strategy.md), and [operations verification](../documentation/operations-runbook.md).
