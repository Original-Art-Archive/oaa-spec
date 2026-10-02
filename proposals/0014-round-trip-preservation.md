<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Meaningful Round-Trip Preservation

## Status

Expanded formal profile deferred from OAA 1.0; core preservation guidance and conformance-claim clarification accepted

## Summary

Defer the expanded formal Round-Trip profile and its demonstration requirement. Retain core preservation recommendations and prohibit unsupported lossless claims, without carrying the ambiguous best-effort Round-Trip category into 1.0.

## Motivation

An archive can remain structurally valid while silently losing unsupported metadata, attachments, or opaque references. A lossless claim needs evidence covering those losses. Defining and demonstrating a complete preservation capability is separate from establishing the core archive contract.

## Specification Changes

For OAA 1.0:

- Define Valid Archive, Structural Reader, Metadata Reader, and Conforming Writer precisely. Do not carry forward the 0.1 best-effort Round-Trip conformance category.
- Implementations SHOULD preserve unknown optional fields, external links, and extension data when rewriting manifests.
- Implementations MUST NOT claim lossless preservation solely because they support both import and export or produce a Valid Archive. Any preservation claim MUST be supported by evidence and state its scope and limitations.
- Deferral MUST NOT weaken any mandatory preservation or other requirement of the core conformance class an implementation claims.
- The expanded profile and its full-preservation demonstration are not prerequisites for the 1.0 release.

### Deferred Profile Scope

Retain the following scope for a future separately specified and testable optional profile, not as current 1.0 obligations:

- Preserve manifest data, including unknown optional fields, external links, and extension data, during unchanged import and export of a valid archive.
- Preserve embedded file bytes and safe unreferenced extra content allowed by the [layout decision](0015-manifest-authority-and-extra-content.md).
- Preserve archive-local identifiers and archive-relative paths, because opaque extensions may refer to them.
- Permit JSON formatting and archive compression changes while preserving data values, relationships, and file contents.
- Limit user-directed edits or exclusions to their intended scope without unrelated silent loss.

## Compatibility Impact

The 1.0 transition removes an ambiguous conformance label without changing the historical 0.1 contract. A later optional preservation profile can establish stronger guarantees without changing what constitutes a Valid Archive under 1.0.

## Examples

A writer can produce a structurally valid archive without proving that an earlier import preserved every unknown field. That result does not establish losslessness. The deferred profile would also test embedded bytes, safe extras, identifiers, paths, and opaque references.

## Open Questions

No open question blocks OAA 1.0. Reconsider the expanded profile when a testable preservation implementation and representative fixtures can demonstrate its proposed scope. Independent implementation exchange remains a separate follow-up milestone.
