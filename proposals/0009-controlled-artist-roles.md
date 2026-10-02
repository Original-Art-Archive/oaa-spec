<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Controlled Artist Credit Roles

## Status

Deferred from OAA 1.0

## Summary

Retain artist credit `role` as display text. Do not introduce a closed role field for OAA 1.0.

## Motivation

Credits may describe combined, specialized, or historically specific contributions. A controlled vocabulary needs shared implementation experience before it can replace or supplement those descriptions.

## Specification Changes

None for OAA 1.0. Existing artist credit objects remain available, and implementations may preserve structured role classifications in namespaced extensions.

## Compatibility Impact

None. A later optional controlled field may be considered using the [base-field design principles](0002-base-field-inclusion-policy.md), without an implementation-count prerequisite.

## Examples

A credit can retain the role "Pencils and inks" without splitting it into invented controlled values.

## Open Questions

No open question blocks OAA 1.0. Reconsider controlled roles when demonstrated unmet credit-interchange needs and community consensus justify a shared model, including how combined contributions should be represented.
