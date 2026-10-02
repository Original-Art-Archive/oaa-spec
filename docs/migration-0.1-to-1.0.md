<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Migrating 0.1 to 1.0

Migration is optional. This checklist explains the accepted transition; [SPEC.md](../SPEC.md) and the [1.0 schema](../schema/1.0/oaa-manifest.schema.json) define the target contract.

Keep the original archive unchanged. A 0.1 archive does not become a 1.0 archive merely by renaming it or changing version strings. Produce a separate output and validate it against the target version.

1. Read the source with explicit 0.1 support. The current reference validator implements 1.0 only; the historical `v0.1.2` snapshot retains the earlier implementation.
2. Preserve the existing base fields and controlled values. Do not invent dimensions, character fields, structured publication fields, monetary structures, or additional vocabulary values.
3. Check all required strings for nonblank values; check actual calendar dates and absolute non-local URLs (or the explicit empty-URL sentinel). Honor nullable fields individually rather than converting nulls wholesale.
4. Verify `size_bytes` against the uncompressed embedded file, positive integer pixel dimensions, and finite positive `dpi_x`/`dpi_y`. Zero-byte files are allowed. Unknown values should be omitted or null only where allowed, not fabricated.
5. Keep collection-authoritative paths and identities. IDs are unique within their own record type; file IDs are scoped to one artwork. A shared external link is not an automatic merge instruction.
6. Preserve provider-native structured information in opaque extension objects where practical. Same-named provider fields and nested provider JSON are allowed; base fields control OAA meaning. Do not flatten structured data into display strings and call it lossless.
7. Package only regular files/directories using Store or Deflate, with safe paths, no encryption, and no file/directory conflicts. Retain safe extras when the migration's stated preservation scope includes them. Do not discover extra records by scanning filenames.
8. Set all authoritative manifests to `schema_version: "1.0"`, emit the exact root `mimetype`, and validate the resulting `.oaa` archive. Report incomplete validation separately from invalidity.

Path-like prose is now advisory rather than automatically invalid. Actual references remain subject to strict safety rules. Private metadata and its extensions remain private; `is_public` is not permission to disclose every attachment or unknown block.

Record what the migration retained, changed, omitted, or could not interpret. A valid output does not by itself prove lossless preservation. No automatic collection migration is supplied by this release.
