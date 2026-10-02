<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposals

Use this directory for proposed OAA format changes.

Proposals should describe compatibility impact and examples before being accepted into the specification.

Use [0000-template.md](0000-template.md) as the starting point for new proposals.

## OAA 1.0 Decision Index

The decisions below are integrated into release 1.0.0. They do not amend the historical 0.1 contract. Deferred fields, capabilities, and processes are not implicit release prerequisites. See the [release verification scope and scope](../docs/release-1.0.0.md).


| Proposal | Decision |
| --- | --- |
| [0001: Versioning transition](0001-1.0-versioning.md) | Accepted; release/schema and patch guarantees clarified |
| [0002: Base-field inclusion policy](0002-base-field-inclusion-policy.md) | Freeze and design principles accepted; formal policy deferred; no implementation-count threshold |
| [0003: Physical artwork dimensions](0003-physical-artwork-dimensions.md) | Deferred |
| [0004: Structured publication details](0004-structured-publication-details.md) | Deferred |
| [0005: Character and subject metadata](0005-character-and-subject-metadata.md) | Deferred |
| [0006: Image role values](0006-image-role-values.md) | Retain existing values; additions deferred |
| [0007: Controlled media category](0007-media-category.md) | Deferred; retain display text |
| [0008: Controlled artwork category](0008-artwork-category.md) | Deferred; retain display text |
| [0009: Controlled artist credit roles](0009-controlled-artist-roles.md) | Deferred; retain display text |
| [0010: Controlled availability](0010-controlled-availability.md) | Deferred; retain display text |
| [0011: Publication status values](0011-publication-status-values.md) | Retain existing values; additions deferred |
| [0012: Archive validity and recovery](0012-archive-validity-and-recovery.md) | Accepted for 1.0 |
| [0013: Independent extension namespaces](0013-extension-namespaces.md) | Accepted for 1.0 |
| [0014: Round-trip preservation](0014-round-trip-preservation.md) | Expanded profile deferred; core guidance and honest claims retained; best-effort category removed from 1.0 |
| [0015: Manifest authority, layout, and extras](0015-manifest-authority-and-extra-content.md) | Accepted for 1.0 |
| [0016: Identifier scope and external links](0016-identifier-scope-and-external-links.md) | Accepted for 1.0 |
| [0017: Existing field values and JSON](0017-primitive-values-and-json.md) | Accepted; DPI field names and nullable-value rules corrected |
| [0018: Archive safety and container support](0018-archive-safety.md) | Accepted for 1.0 |
| [0019: Privacy and publication intent](0019-privacy-and-publication.md) | Accepted for 1.0 |
| [0020: Integration and release gates](0020-1.0-release-gates.md) | Core release preparation complete; exchange, full preservation, and MIME registration are follow-ups |

Each proposal records its own compatibility impact and examples. The [open-question record](../OPEN_QUESTIONS.md) distinguishes completed design review from deferred follow-up work.
