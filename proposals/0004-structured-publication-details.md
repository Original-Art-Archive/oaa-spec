<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Structured Publication Details

## Status

Deferred from OAA 1.0

## Summary

OAA 1.0 does not define structured base fields for publication details such as publisher, series title, issue number, page number, publication date, or publication year.

## Motivation

These concepts may be portable, but current repository evidence shows the structured fields only in SNIKT.com extension examples. Other documented mappings share only the broader OAA `publication_status` field.

Publication metadata also varies across covers, interior pages, comic strips, trading cards, animation art, and other artwork types. OAA should not standardize one provider's model before community and implementation consensus establishes a shared structure.

## Specification Changes

None for OAA 1.0. Implementations may preserve structured publication details in namespaced extension blocks. The existing base `publication_status`, `title`, `description`, and `artwork_type` fields remain unchanged.

## Compatibility Impact

None. A later OAA 1.x revision may add optional structured publication fields if demonstrated need, community consensus, and the [base-field design principles](0002-base-field-inclusion-policy.md) justify them. Retaining publication text in a title or description is not equivalent to preserving existing structured source data in extensions.

## Examples

The existing [SNIKT.com extension examples](../docs/provider-extensions.md#public-and-private-metadata) demonstrate provider-specific publisher, series, issue, page, and date fields.

## Open Questions

No open question blocks OAA 1.0. Reconsider shared publication fields when concrete unmet interchange needs and community consensus establish suitable semantics. Character or subject metadata is outside this proposal and requires a separate decision.
