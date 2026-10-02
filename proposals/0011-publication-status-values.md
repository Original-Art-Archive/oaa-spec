<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Publication Status Values for OAA 1.0

## Status

Accepted for OAA 1.0; additional values deferred

## Summary

Retain only `published_art` and `unpublished_art` as base `publication_status` values for OAA 1.0.

## Motivation

Additional publication states need shared definitions. An uncertain or more specialized source value should not be forced into a misleading base value.

## Specification Changes

- The existing closed value set remains unchanged.
- Writers MAY omit the optional `publication_status` field when the status is unknown or cannot be mapped faithfully.
- Writers MAY preserve source-specific publication information in namespaced extensions.

## Compatibility Impact

No vocabulary change for OAA 1.0. Under the [versioning decision](0001-1.0-versioning.md), adding a closed-set value after 1.0 requires a new major schema version.

## Examples

An artwork with uncertain publication history can omit `publication_status` while retaining that uncertainty in descriptive text or an extension.

## Open Questions

No open question blocks OAA 1.0. Additional values are deferred pending community consensus. [Structured publication details](0004-structured-publication-details.md) remain a separate deferred proposal.
