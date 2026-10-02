<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Image Role Values for OAA 1.0

## Status

Accepted for OAA 1.0; additional values deferred

## Summary

Retain the existing closed `image_role` value set for OAA 1.0.

## Motivation

The existing roles cover the established interchange cases. Additional roles need community evidence before becoming part of a closed vocabulary.

## Specification Changes

- The base values remain `raw_scan`, `raw_photo`, `corrected_scan`, `detail`, `verso`, and `reference`.
- Writers MAY omit the optional `image_role` field when no base value fits and preserve more specific classification in a namespaced extension.
- This decision does not change the required `file_kind` field or its existing `raw`, `derivative`, and `supporting` values.

## Compatibility Impact

No vocabulary change for OAA 1.0. Under the [versioning decision](0001-1.0-versioning.md), adding a closed-set value after 1.0 requires a new major schema version.

## Examples

A reverse-side scan can use `verso`. A provider-specific image classification with no matching base role can be retained in an extension without assigning a misleading base role.

## Open Questions

No open question blocks OAA 1.0. Additional roles are deferred pending community consensus and concrete interchange cases.
