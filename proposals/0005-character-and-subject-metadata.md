<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Character and Subject Metadata

## Status

Deferred from OAA 1.0

## Summary

OAA 1.0 does not define dedicated base fields for characters or other depicted subjects.

## Motivation

The current base format can capture descriptive character or subject information in an artwork's `title`, public `description`, or private `personal_notes`. Existing structured source data belongs in namespaced extension blocks; flattening it into those text fields is not equivalent preservation.

A portable subject model would require community consensus on whether it covers characters only or also people, locations, objects, franchises, and other subjects; whether multiple values are allowed; and whether values are display text or stable external identities.

## Specification Changes

None for OAA 1.0. The existing `title`, `public_metadata.description`, and `private_metadata.personal_notes` fields remain unchanged.

## Compatibility Impact

None. A later OAA 1.x revision may add optional character or subject fields if demonstrated need, community consensus, and the [base-field design principles](0002-base-field-inclusion-policy.md) justify them.

## Examples

Character information may be captured without a dedicated base field:

```json
{
  "title": "Example Hero Commission",
  "public_metadata": {
    "description": "A commission depicting the Example Hero."
  }
}
```

## Open Questions

No open question blocks OAA 1.0. Reconsider when concrete unmet subject-metadata needs and community consensus justify a shared model.
