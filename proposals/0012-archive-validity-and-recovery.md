<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Archive Validity and Reader Recovery

## Status

Accepted for OAA 1.0

## Summary

Separate archive validity, recoverable content, advisory warnings, unsupported versions, reader capacity limits, and destination-specific limitations.

## Motivation

A reader can recover useful records from a damaged archive without making that archive valid. Conversely, a valid archive may exceed a particular reader's configured capacity.

## Specification Changes

- A violation of an archive-level `MUST` or `MUST NOT` requirement makes the archive invalid.
- Readers MAY recover usable content from invalid input, subject to all safe-reading requirements. Recovery functionality is optional and MUST NOT be a core conformance prerequisite.
- Readers performing recovery MUST identify the input as invalid and report known skipped or lost content; recovery MUST NOT be presented as a complete conforming import.
- A referenced embedded file that is missing makes the archive invalid, even when artwork metadata can be recovered.
- Failure to meet a `SHOULD` recommendation produces an advisory warning rather than invalidity solely on that basis.
- Exceeding a reader's configured resource limits MUST be reported as an inability to process the input, not as proof that the archive violates the format.
- Readers MUST distinguish unsupported schema versions and destination-specific extraction limitations from archive invalidity. An unsupported version alone does not establish whether the archive conforms to that version's contract.
- Readers that stop before completing validation MUST NOT report the archive as fully validated.

## Compatibility Impact

OAA 1.0 conformance language, validator severity, and diagnostics must agree on these distinctions. Recovery does not relax the valid-archive contract.

## Examples

A missing scan makes an archive invalid, but a reader can salvage the title and credits with an explicit recovery report. A `mimetype` entry that is not first produces a warning. A configured archive-size limit can stop reading without establishing invalidity.

## Open Questions

None for this decision. Diagnostic alignment and fixtures remain part of the 1.0 integration work.
