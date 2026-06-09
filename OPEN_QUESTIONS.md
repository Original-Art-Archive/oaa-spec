<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Open Questions

Open questions are expected for the OAA 0.1 Draft. They must be resolved or explicitly deferred before a 1.0 release.

## Controlled Values and Display Strings

`file_kind`, `image_role`, and `publication_status` currently use closed base value sets.

Open points:

- Whether future revisions should add more `image_role` base values.
- Whether `public_metadata.media` should remain display text or gain a separate closed `media_category` field.
- Whether `public_metadata.artwork_type` should remain display text or gain a separate closed `artwork_category` field.
- Whether artist credit roles should remain display text or gain a separate closed role field.
- Whether `for_sale_status` should be supplemented by a closed `availability` field.
- Whether future revisions should add more `publication_status` base values beyond `published_art` and `unpublished_art`.

## Conformance Fixtures

The 0.1 conformance fixture set should remain focused on the core archive contract:

- minimal valid archive
- multi-gallery artwork
- invalid path traversal
- duplicate archive entry
- unknown extension preservation
