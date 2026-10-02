<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Open Questions

The reviewed pre-1.0 design questions have been resolved or explicitly deferred in the [proposal decision index](proposals/README.md). Accepted changes are integrated into release 1.0.0. The historical 0.1 contract remains unchanged.

## Controlled Values and Display Strings

The existing `file_kind` values remain unchanged. Each previously open vocabulary question now has a separate decision:

- [Image roles](proposals/0006-image-role-values.md): retain the six existing values; defer additions.
- [Media category](proposals/0007-media-category.md): defer a controlled field; retain `media` display text.
- [Artwork category](proposals/0008-artwork-category.md): defer a controlled field; retain `artwork_type` display text.
- [Artist credit roles](proposals/0009-controlled-artist-roles.md): defer controlled roles; retain display text.
- [Availability](proposals/0010-controlled-availability.md): defer a controlled field; retain `for_sale_status` display text.
- [Publication status](proposals/0011-publication-status-values.md): retain `published_art` and `unpublished_art`; defer additions.

Physical dimensions, structured publication details, structured character or subject metadata, and structured monetary values remain deferred. Reconsider them when concrete unmet needs and community discussion justify shared semantics, using the [base-field design principles](proposals/0002-base-field-inclusion-policy.md), examples, and compatibility analysis. No minimum implementation count is required. Existing structured source data belongs in extensions; flattening it into display text is not equivalent preservation.

See the [release notes](docs/release-1.0.0.md) for verification scope and limitations.

## Deferred and Follow-Up Work

These items do not block 1.0.0:

- [Formal field-admission policy](proposals/0002-base-field-inclusion-policy.md): revisit when actual proposals demonstrate a need for more governance. The existing base field set is frozen for 1.0.
- [Expanded preservation profile](proposals/0014-round-trip-preservation.md): revisit when a testable implementation and representative fixtures can demonstrate the intended guarantee. Full preservation of safe extras, IDs, and paths belongs to this deferred scope, not an implied core guarantee.
- Independent implementation exchange: arrange when another implementation is available for testing; do not claim demonstrated independent interoperability before then.
- MIME registration: prepare and submit when the registration material is ready. Submission and approval are tracked administrative work, not release blockers.

These deferrals do not weaken archive integrity, safety, privacy, or mandatory requirements of a claimed core conformance class. No named application or minimum number of independent adopters is required for release.
