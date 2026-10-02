<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Identifier Scope and External Links

## Status

Accepted for OAA 1.0

## Summary

Define identifiers by record type and archive-local scope. Treat external links as associations rather than automatic merge keys.

## Motivation

Different record types can safely reuse the same opaque identifier. Multiple artworks can also legitimately refer to the same external publication or source record.

## Specification Changes

- Gallery IDs MUST be unique among galleries in an archive, and artwork IDs MUST be unique among artworks in that archive.
- File IDs MUST be unique within their containing artwork.
- The same identifier string MAY occur in different record-type scopes or as a file ID in different artworks.
- OAA IDs are opaque and archive-local; the format does not assert global identity or stability across independent exports. A stronger unchanged-import/export preservation guarantee belongs to the [deferred preservation profile](0014-round-trip-preservation.md), not the 1.0 core identity contract.
- Readers MUST NOT treat a shared external link alone as an instruction to merge artwork records.
- Readers MUST NOT reject unknown external link providers.
- Redundant external links MAY produce a warning, but repetition alone MUST NOT make an archive invalid. Do not impose archive-wide uniqueness on a `(provider, id)` pair.

## Compatibility Impact

This clarifies ambiguous identifier-uniqueness language. It supersedes the earlier review suggestion to reject duplicate external provider-and-ID pairs; that rejection is not part of the accepted design.

## Examples

A gallery and artwork can both use `001`. Two artworks can link to the same publication record while remaining distinct artworks. Repeating an identical external link is redundant, not an identity collision.

## Open Questions

None for this decision. Cross-archive matching and application merge policies remain outside the base format.
