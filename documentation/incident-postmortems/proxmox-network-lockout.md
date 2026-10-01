# Incident — Proxmox static address on the wrong subnet

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and evidence

The Proxmox web interface was unreachable after installation. The configured static address was on `192.168.1.x`, while the home LAN and management client used `192.168.0.x`.

These private ranges describe the mismatch; they are not an instruction to substitute an arbitrary host address.

## Investigation and fix

Physical console access provided a path to inspect the host without relying on the failed management connection. The configured address was compared with the client's network information and the intended LAN/gateway.

The static configuration in `/etc/network/interfaces` was corrected to the actual home network. Reachability and management access were then checked before continuing application setup.

## Cause and result

The known cause for this installation episode was a static IP/subnet mismatch. Correcting the configuration restored management access.

The later [reboot outage](proxmox-reboot-outage.md) is documented separately. The similar symptoms do not establish that every later outage had the same cause.

## Lesson

Record the intended address, prefix, gateway, and management path before installing a headless host. Maintain physical console access, and verify address persistence after a planned reboot before depending on remote management.
