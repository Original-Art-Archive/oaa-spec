<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Privacy and Publication Intent

## Status

Accepted for OAA 1.0

## Summary

Keep private metadata private by default and distinguish artwork visibility intent from authorization to publish associated data or files.

## Motivation

An archive can contain public artwork descriptions alongside private notes, acquisition details, receipts, and opaque provider data. Importing a visible artwork must not disclose all of that content automatically.

## Specification Changes

- Private metadata, including extensions within private metadata, MUST remain private by default.
- `is_public` expresses visibility intent for the record; it MUST NOT be interpreted as blanket authorization to publish private metadata, notes, receipts, or attachments.
- Readers MUST NOT automatically publish unknown extension contents or supporting files merely because the containing artwork is public.
- Publishing private or otherwise unclassified content requires explicit user authorization appropriate to that content.
- This decision introduces no new privacy field. Preservation of content during interchange does not authorize its publication.

## Compatibility Impact

This clarifies reader and publisher behavior without expanding the base metadata model. Implementations must keep import, preservation, and public display decisions separate.

## Examples

A public artwork can retain a private acquisition note and receipt inside the archive. Importing that artwork does not authorize publishing the note, receipt, or an unknown provider extension.

## Open Questions

None for this decision. More granular shared privacy metadata would require a separate future proposal.
