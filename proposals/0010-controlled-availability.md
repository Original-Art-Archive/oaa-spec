<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Controlled Availability

## Status

Deferred from OAA 1.0

## Summary

Retain `for_sale_status` as display text. Do not introduce a closed `availability` base field for OAA 1.0.

## Motivation

Sale, trade, reservation, and ownership states do not yet have a community-agreed interchange model. Display text preserves current intent without assigning stronger transactional meaning.

## Specification Changes

None for OAA 1.0. Implementations may preserve structured availability information in namespaced extensions.

## Compatibility Impact

None. A later optional base field may be considered using the [base-field design principles](0002-base-field-inclusion-policy.md), without an implementation-count prerequisite.

## Examples

`for_sale_status` can retain "Open to trades" without requiring a new controlled availability value.

## Open Questions

No open question blocks OAA 1.0. Controlled availability is deferred pending community consensus and demonstrated interchange needs.
