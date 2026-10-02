<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Controlled Artwork Category

## Status

Deferred from OAA 1.0

## Summary

Retain `public_metadata.artwork_type` as display text. Do not introduce a closed `artwork_category` base field for OAA 1.0.

## Motivation

Artwork classifications overlap and vary between collections and platforms. The existing display field preserves useful descriptions without imposing an unproven shared taxonomy.

## Specification Changes

None for OAA 1.0. Implementations may preserve structured artwork classifications in namespaced extensions.

## Compatibility Impact

None. A later optional base field may be considered using the [base-field design principles](0002-base-field-inclusion-policy.md), without an implementation-count prerequisite.

## Examples

`artwork_type` can retain "Cover prelim" without requiring agreement on whether it belongs to a cover, preliminary-art, or sketch category.

## Open Questions

No open question blocks OAA 1.0. A controlled category is deferred pending community consensus and concrete interchange needs.
