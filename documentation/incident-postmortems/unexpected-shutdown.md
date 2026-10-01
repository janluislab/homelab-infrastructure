# Incident — Unexpected Proxmox shutdown during laptop handling

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and evidence

Proxmox intermittently became unreachable. The initial suspicion was lid handling or sleep behavior, but the journal contained:

```text
systemd-logind: Power key pressed short
```

The laptop's physical power button was being pressed while closing/handling the lid. The resulting clean shutdown explained the disappearance of the host and its services.

## Investigation and fix

`journalctl` distinguished a deliberate shutdown event from a network failure or a suspended host. Power-key handling was changed to `HandlePowerKey=ignore` alongside the existing lid-switch handling.

This describes the recorded configuration change; no new host power policy was applied during the documentation update.

## Result and lesson

The short-press shutdown trigger was identified and addressed. An extended uptime benchmark was not recorded, so this report does not claim a measured availability improvement.

Repurposed laptop hosts have hardware and power-management behavior that must be checked. Examine shutdown events before changing network or storage settings in response to an unreachable host.

Reference: [systemd logind configuration](https://www.freedesktop.org/software/systemd/man/latest/logind.conf.html).
