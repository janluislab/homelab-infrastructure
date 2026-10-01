# Incident — Windows metadata warnings during restore

[All incidents](README.md) · [Portfolio](../../README.md)

## Observations

Restores produced messages including `set named security info failed: Access is denied` and `unsupported file type irregular` while Windows-origin data was being restored to a Linux/NAS destination.

## Investigation

Windows security metadata cannot be assumed to map exactly to the destination's ownership and ACL model. A metadata warning and a skipped/unsupported object are also different conditions; they should not all be dismissed as cosmetic.

Validation used file counts, total restored size, opening representative files, and application checks to assess the recovered content independently of the warning count.

## Result and limits

The restored content used by the migrated application was checked and functional. The record does not establish a perfect reproduction of all NTFS security descriptors or an exhaustive comparison of every special file and metadata field.

The original incident notes treated much of the output as metadata portability noise. This report keeps that finding bounded to the content actually checked.

## Lesson

Separate content recovery, metadata portability, and skipped objects when reading restore logs. Investigate exceptions and retain the validation scope rather than claiming that any successful application test proves a complete restore of every object.
