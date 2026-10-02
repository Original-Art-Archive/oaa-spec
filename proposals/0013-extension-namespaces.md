<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Independent Extension Namespaces

## Status

Accepted for OAA 1.0

## Summary

Treat each object-valued, namespaced extension block as independent provider data. Base fields remain authoritative for OAA interpretation.

## Motivation

Rejecting provider keys because their names also occur in the base model creates accidental coupling. Ordinary nested provider JSON should not inherit unrelated base-field rules.

## Specification Changes

- Extension namespace values MUST be JSON objects.
- Writers MUST place newly created provider-specific or implementation-specific fields in namespaced extension blocks, using reverse-DNS namespace names.
- A key inside an extension MAY have the same name as a present base field; the base field MUST remain authoritative for OAA interpretation.
- Extension objects MAY contain ordinary nested JSON, including a nested key named `extensions`. That key has no OAA extension-container meaning merely because of its name.
- Readers MUST ignore unrecognized optional fields and unknown extension blocks when interpreting the base format.
- Implementations SHOULD preserve unknown received fields, external links, and extension data when rewriting manifests. The stronger formal preservation profile is [deferred](0014-round-trip-preservation.md), not a 1.0 conformance requirement; mandatory duties of a claimed core conformance class still apply.
- Replace the existing extension base-field-shadowing and nested-extension-container prohibitions for OAA 1.0.

## Compatibility Impact

This changes the 0.1 extension restrictions. It is part of the explicit 1.0 transition, not a retroactive change to 0.1 validation.

## Examples

An artwork's base `title` controls its OAA title even if a provider extension also contains `title`. A provider's nested `extensions` object remains ordinary provider data.

## Open Questions

None for this decision. Spec, schema, and validator restrictions must be aligned during integration.
