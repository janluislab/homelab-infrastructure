# Incident — NFS permissions across TrueNAS, Proxmox, and LXC

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact

The restored TrueNAS dataset could not be written to reliably from the Proxmox/Immich environment. One permission change was not enough because multiple identity and ACL layers were involved.

## Investigation

The diagnostic approach tested a direct mount and file creation on the Proxmox host before testing through LXC and Docker. This separated export/dataset failures from container identity failures.

| Blocking layer | Observation | Recorded change |
|---|---|---|
| NFS root mapping | Host-root access was affected by the export's root identity handling | Adjusted TrueNAS Maproot user/group settings |
| Group ownership | Files did not belong to the expected mapped group | Matched ownership/group access to the export identity |
| Existing dataset ACLs | New permissions had not reached restored subdirectories | Applied permissions recursively to existing content |
| Unprivileged LXC identity | Container root mapped to a high host UID rather than host UID 0 | Added a broad Everyone/Full Control dataset ACL as a lab workaround |

The restore included roughly 80,000 existing files. A permission change on only the parent directory could therefore leave a substantial part of the library inaccessible.

## Cause

Access was evaluated using different effective identities at the host, NFS server, and container boundaries. Unprivileged LXC root is not necessarily host UID 0; export mapping, file ownership, and ACL inheritance must all be checked against the identity reaching the dataset.

## Result and limitation

The changes restored the required access, and the application could use the NFS-backed media. The resulting broad ACL was accepted during lab recovery but is **not least privilege**. Client/subnet restriction does not remove the risk of excessive permissions on an authorized host.

The exact final export mapping is not captured in the current screenshot evidence, so this report does not publish an invented configuration.

## Next hardening step

Replace the broad ACL with a dedicated service identity, explicit UID/GID mapping, restricted clients, and the minimum dataset permissions. Test both existing media reads and new writes before removing the workaround.

See [architecture paths](../../architecture/infrastructure-diagram.md) and [security limitations](../../security/controls-and-limitations.md).
