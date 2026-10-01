# Incident — External-drive errors during backup copying

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and evidence

A local external drive encountered errors while copying backup data. Windows `chkdsk` reported bad sectors/cluster remapping in the build record.

## Recovery and validation

Filesystem checking recovered enough readability to continue the migration work. The backup was checked again rather than being trusted only because the copy completed.

## Cause and confidence

The observed bad-sector report is evidence of a problem with that backup medium. It does not establish that software repair healed the physical disk. Cluster remapping and restored readability are narrower outcomes than hardware reliability.

The record does not establish a complete long-term replacement history for this device, so this report does not claim that the affected disk remained a dependable backup repository.

## Lesson

Treat a backup device as another failure-prone component. Preserve independent recovery copies, verify repository consistency, test a restore, and retire unreliable media from roles where it would be the only recovery path.

Related: [Restic strategy](../../backup/restic-strategy.md). This backup-drive event is separate from the later [TrueNAS pool investigation](truenas-storage-hdd.md); the evidence does not identify them as the same device failure.
