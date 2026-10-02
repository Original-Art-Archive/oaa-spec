<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Physical Artwork Dimensions

## Status

Deferred from OAA 1.0

## Summary

OAA 1.0 does not define a base field for an artwork's physical dimensions.

## Motivation

Physical dimensions have provider-neutral meaning, but current interchange evidence does not justify a base field. Collectors may record metric measurements, measurements in inches, named page sizes such as Letter or A3, approximate measurements, or measurements of irregular artwork.

The existing `files[].width` and `files[].height` fields describe embedded-file pixel dimensions only.

## Specification Changes

None for OAA 1.0. Implementations may preserve physical measurements or named page sizes in namespaced extension blocks.

## Compatibility Impact

None. A later OAA 1.x revision may add an optional base field if demonstrated need, community consensus, and the [base-field design principles](0002-base-field-inclusion-policy.md) justify it.

## Examples

An implementation may preserve a display-oriented value in its extension block. Existing structured source measurements belong in extensions as structured data; flattening them to this display string is not equivalent preservation.

```json
{
  "public_metadata": {
    "extensions": {
      "app.example-reader": {
        "physical_dimensions": "297 × 420 mm (A3)"
      }
    }
  }
}
```

## Open Questions

No open question blocks OAA 1.0. Reconsider when demonstrated unmet measurement needs and community discussion justify shared semantics; compare display-oriented and structured representations without an implementation-count prerequisite.
