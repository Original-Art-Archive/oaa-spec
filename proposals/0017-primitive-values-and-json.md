<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Existing Field Values and JSON Validation

## Status

Accepted for OAA 1.0

## Summary

Tighten the meaning and validation of existing base fields without adding new metadata fields.

## Motivation

Shape-only validation permits impossible sizes and dates, empty required identifiers, and parser-dependent JSON. These ambiguities affect reliable interchange even without expanding the field model.

## Specification Changes

- When present and non-null, `size_bytes` MUST be an integer greater than or equal to zero and MUST equal the embedded file's uncompressed byte length.
- When present and non-null, pixel `width` and `height` MUST be positive integers, and each of `dpi_x` and `dpi_y` MUST be a finite positive number. These are the existing horizontal and vertical DPI fields; this decision adds no combined DPI field.
- Pixel and DPI constraints validate the supplied metadata. OAA conformance MUST NOT require decoding or rendering embedded media or installing image codecs to verify those values against media contents.
- When present and non-null, base date-only fields MUST contain real calendar dates in `YYYY-MM-DD` form, not merely strings matching that shape.
- Required names, titles, and identifiers MUST NOT be empty or whitespace-only.
- Writers SHOULD omit optional fields whose values are unavailable. `null` MUST be used only where the field definition explicitly permits it; allowed nulls remain valid and are exempt from the numeric and date-value constraints above.
- An external link `url` MUST be either the existing empty-string sentinel or a valid absolute URL that is not a local-file reference. Readers MUST NOT automatically fetch or execute a URL merely because it appears in a manifest.
- Price and estimate fields remain display strings. Structured amounts and currencies are deferred; this decision introduces no new fields or value sets.
- Manifests MUST be valid JSON. Readers MUST reject duplicate object member names and non-JSON numeric tokens such as `NaN` or `Infinity`.
- Base-field value rules MUST NOT be imposed on unrelated, same-named keys inside provider extensions.

## Compatibility Impact

Some values accepted by permissive 0.1 readers or schemas will not meet 1.0 requirements. These checks belong in the explicit 1.0 contract, with schema checks supplemented by archive-aware validation where needed.

## Examples

`2024-02-29` is a valid date; `2025-02-29` is not. An empty embedded file can have `size_bytes` of zero, but an image dimension of zero is invalid. `dpi_x: null` represents an unknown value where null is allowed; `dpi_y: 300` is a valid positive DPI value. A price such as "Price on request" remains display text.

## Open Questions

No open design question blocks integration. Structured monetary metadata requires a separate future proposal and community evidence.
