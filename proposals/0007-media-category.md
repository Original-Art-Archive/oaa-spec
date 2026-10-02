<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Controlled Media Category

## Status

Deferred from OAA 1.0

## Summary

Retain `public_metadata.media` as display text. Do not introduce a closed `media_category` base field for OAA 1.0.

## Motivation

A shared classification needs community agreement on mixed media, overlapping techniques, and terminology. Display text already preserves the collector's description without requiring that agreement.

## Specification Changes

None for OAA 1.0. Implementations may preserve structured media classifications in namespaced extensions.

## Compatibility Impact

None. A later optional base field may be considered using the [base-field design principles](0002-base-field-inclusion-policy.md), without an implementation-count prerequisite.

## Examples

`media` can retain a description such as "Ink and watercolor on paper" without mapping it to a single category.

## Open Questions

No open question blocks OAA 1.0. A controlled category is deferred pending community consensus and demonstrated unmet classification needs.
