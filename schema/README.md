<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Schema

This directory contains the normative JSON Schema for OAA 0.1 dot-manifest files.

[oaa-manifest.schema.json](oaa-manifest.schema.json) is normative for manifest-local JSON structure: field presence, JSON types, and constraints expressible within a single manifest document.

[../SPEC.md](../SPEC.md) remains normative for archive layout, cross-file integrity, ZIP behavior, security, privacy, and semantic reader/writer requirements.

The [reference validator](https://github.com/Original-Art-Archive/oaa-validator) loads this schema with `jsonschema` for manifest-local checks, then applies additional archive-wide and cross-reference validation rules.

The schema reflects the 0.1 base-field portability model:

- Closed base value sets are enforced for `public_metadata.publication_status`, `files[].file_kind`, and `files[].image_role`.
- Constrained strings, such as IDs, paths, provider identifiers, media types, URLs, and dates, are validated by syntax or structure where JSON Schema can express that validation.
- Display-oriented strings, such as `public_metadata.media`, `public_metadata.artwork_type`, `artist_credits[].role`, `public_metadata.for_sale_status`, and `files[].format`, are represented as strings or null values and are not treated as controlled OAA machine values.
- Provider-specific and application-specific values should be preserved in extension blocks, not written into closed base fields.
