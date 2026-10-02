<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Changelog

## 1.0.0 — 2026-10-01

- Fixes the Deflate EOF hang and moves version dispatch ahead of 1.0-specific container checks, retaining bounded root probing and 1.0 compression rejection. The normative contract is unchanged.
- Integrates the accepted pre-1.0 decisions; manifests use `"1.0"` with a versioned schema. Historical 0.1 schema/catalog remain unchanged.
- Freezes existing base fields and closed values; defers new structured metadata and the expanded preservation profile.
- Clarifies root-manifest authority, optional layouts and empty structures, safe extras, ID scopes, unknown providers, and opaque independent extension blocks.
- Tightens numeric values, real dates, absolute non-local URLs, nonblank required strings, JSON parsing, and actual embedded byte-size checks.
- Distinguishes invalid input from advisory findings, unsupported versions, and incomplete processing.
- Requires Store/Deflate, bounded ZIP64 processing, safe entry types and path relationships, extraction collision protection, and private-by-default handling.
- Defines the four core conformance classes and removes the ambiguous best-effort Round-Trip class; preservation claims need scoped evidence.
- Aligns the reference validator, conformance fixtures, traceability, examples, migration notes, and release verification.

## 0.1.2

- Updates maintainer, copyright, and mark-policy references to Remgrandt Works.
- Adds the trade-name notice to the license.

## 0.1.1

- Adds GitHub Pages configuration for the public specification site.
- Adds file catalog pages for example folders so dot-manifest fixtures are reachable from the site navigation.
- Updates repository navigation links so Markdown documentation is reachable from the root README.

## 0.1 Public Draft

- Initial public draft release of the Original Art Archive (OAA) Format.
- Defines `.oaa` as a ZIP-compatible archive container with a required root `mimetype` file using `application/vnd.original-art-archive+zip`.
- Defines collection, gallery, and artwork manifest structures using dot-manifest files.
- Defines `galleries/` for gallery manifests and `artworks/` for artwork manifests, with gallery entries referencing artwork records by ID.
- Defines manifest `schema_version` as the string `"0.1"` and documents reader behavior for unsupported versions.
- Defines shared base metadata, external links, and reverse-DNS extension blocks for portable and provider-specific metadata.
- Defines archive path safety rules, duplicate archive entry handling, JSON duplicate member handling, reference object requirements, and privacy rules for `private_metadata`.
- Documents reader expectations for explicit ZIP directory entries, external link display behavior, duplicate gallery membership, display order, primary-file fallback, unsupported media preservation, local path safety, and conformance levels.
- Provides field, type, and required-member definitions for nested complex object types, including external links, collection references, gallery artwork references, and artist credits.
- Allows artwork `public_metadata` to be omitted and allows `artist_credits` to be omitted when no artist data is known.
- Includes 0.1 decisions for identifiers, date formats, controlled values, embedded-file conformance, local path rejection, and conformance fixture scope.
- Includes example archives, manifest fixtures, normative manifest JSON Schema, requirements traceability, and proposal template.
