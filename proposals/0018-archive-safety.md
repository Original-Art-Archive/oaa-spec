<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Archive Safety and Container Support

## Status

Accepted for OAA 1.0

## Summary

Define a portable container subset and mandatory safe-reading behavior while keeping archive validity distinct from destination and resource limitations.

## Motivation

Independent readers need a shared compression contract, safe extraction behavior, and bounded processing. Privacy heuristics for prose should not be confused with unsafe archive paths.

## Specification Changes

- Archive entries MUST use Store or Deflate compression. Encryption MUST NOT be used.
- ZIP64 archives MUST be supported by conforming readers within their configured resource limits; exceeding a limit follows the [capacity diagnostic rule](0012-archive-validity-and-recovery.md).
- Archive entry paths MUST NOT be absolute or contain parent-directory traversal. Duplicate entry paths, symbolic links, special filesystem entries, and file-versus-directory path conflicts MUST be rejected.
- Archive path identities remain case-sensitive.
- Before writing extracted content, readers MUST detect destination path collisions, including those caused by case or Unicode normalization, and MUST NOT silently overwrite colliding content. A destination-specific collision does not by itself make distinct archive paths identical or invalid.
- Conformance does not require extraction onto every filesystem. Destination limitations MUST be reported without claiming successful extraction or full validation when those operations have not completed.
- Readers MUST enforce bounded reads and decompression using declared resource limits. No single universal size limit is imposed by this decision.
- Readers MAY preserve embedded media without decoding it; preserving a file does not require rendering its contents.
- Unsafe archive and file references MUST be rejected. Replace the blanket rejection of apparent local paths in arbitrary text with privacy warnings for path-like prose; such prose alone MUST NOT make the archive invalid.

## Compatibility Impact

Store and Deflate become the required compression subset rather than a recommendation. Safety, ZIP64, resource-limit, and text-validation changes must be reflected consistently in the 1.0 spec and validator without changing historical 0.1 artifacts.

## Examples

`../scan.png` is an invalid archive path. Distinct `Scan.png` and `scan.png` entries require collision handling on a case-insensitive destination. A private note mentioning a former local folder can trigger a privacy warning without invalidating the archive.

## Open Questions

None for this decision. Exact fixtures and portable diagnostics remain integration work; resource budgets remain implementation-configurable.
