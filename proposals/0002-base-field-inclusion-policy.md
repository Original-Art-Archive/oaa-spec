<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: OAA Base-Field Inclusion Policy

## Status

Base-field freeze and design principles accepted for OAA 1.0; formal admission policy deferred

## Summary

Freeze the existing base field set for OAA 1.0. Retain provider-neutrality and demonstrated-need principles for future proposals without a mandatory implementation-count threshold.

## Motivation

The OAA base model should carry shared meaning without standardizing speculative fields or importing one provider's vocabulary. Namespaced extension blocks remain the home for provider-specific, application-specific, and experimental data.

## Specification Changes

No new base fields are introduced for OAA 1.0. A formal field-admission process is deferred; the following principles guide future proposals:

- Define stable, provider-neutral semantics without adopting a provider-specific controlled vocabulary.
- Demonstrate an unmet interchange or archive-integrity need that existing base fields, external links, and file entries do not already represent.
- Provide concrete examples and compatibility analysis.
- Consider evidence from multiple implementations when available, without requiring a minimum number of implementations or adopters.

Provider-specific, experimental, and not-yet-standardized structured data belongs in namespaced extensions. Flattening that data into display text is not equivalent preservation.

Candidate fields still require separate decisions. The earlier mandatory two-implementation threshold is withdrawn, not retained as a condition on future consideration.

## Compatibility Impact

None for the frozen 1.0 field set. Future additions follow the [versioning decision](0001-1.0-versioning.md): compatible optional fields may be added in OAA 1.x, while new required fields or changes to closed base value sets require a new major schema version.

## Examples

A provider can retain structured measurements in its extension now and later propose a shared field with clear semantics, examples, and compatibility analysis. A second implementation is useful evidence, not a prerequisite for considering the proposal.

## Open Questions

No open question blocks OAA 1.0. Revisit a formal admission process when actual field proposals demonstrate a need for additional governance; reconsider individual deferred fields when concrete unmet metadata needs and community discussion justify it.
