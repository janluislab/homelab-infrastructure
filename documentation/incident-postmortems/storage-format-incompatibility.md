# Incident — Windows Storage Spaces disk mistaken for plain NTFS

[All incidents](README.md) · [Portfolio](../../README.md)

## Impact and evidence

A source disk expected to be readable as NTFS could not be mounted through the planned Linux/TrueNAS path. Inspection with `blkid` and `file -s` identified a `Storage pool` partition label and a `SPACEDB` signature.

## Cause and decision

The disk had been configured under Windows Storage Spaces. It was not simply a conventional NTFS data partition available to the attempted mount workflow.

The migration changed to a known-compatible plain-NTFS source instead of forcing unsupported modifications onto the TrueNAS appliance or reformatting the only recovery source.

## Result and lesson

A compatible source allowed the transfer to continue. No claim is made that the Storage Spaces disk was converted in place or natively supported by the chosen TrueNAS workflow.

Identify the storage layout before treating a mount failure as data loss. Preserve the source and choose a supported recovery path rather than experimenting destructively with the only copy.
