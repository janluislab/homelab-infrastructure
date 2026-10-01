# Incident — Proxmox management unavailable after a reboot

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and observations

A later troubleshooting session reported that the Proxmox address responded to ping while the web interface was unavailable after a reboot. An unexpected address on `192.168.1.x` was also noticed compared with the intended `192.168.0.x` network.

Proxmox access was subsequently restored. The complete service logs and exact final recovery commands for this episode were not retained in the evidence reviewed for the portfolio.

## Diagnostic boundaries

ICMP reachability established only that an address was reachable. It did not verify the Proxmox management service, the identity of the responding host, NFS availability, or Immich health.

The appropriate checks separate network configuration from the management services and dependent containers. The [operations runbook](../operations-runbook.md) includes address/route checks, `pveproxy`/`pvedaemon` status, storage mount checks, and application health.

## Outcome and confidence

Management availability was reported restored, and later application output confirmed the Immich stack running. The observed address mismatch was relevant, but the retained record does not establish the exact service-level root cause or prove that this was identical to the original installation lockout.

## Lesson

After a reboot, verify the host, management interface, remote mounts, containers, and application separately. A recovered login page and a successful ping are useful observations, not a complete recovery test.
