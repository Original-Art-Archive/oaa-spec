<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Versioning

This page explains [SPEC.md](../SPEC.md); it does not add requirements.

Specification release **1.0.0** uses manifest `schema_version: "1.0"`.

Release tags use `MAJOR.MINOR.PATCH`. Manifest compatibility identifiers use `MAJOR.MINOR`; they are exact identifiers, not strings to sort to decide support. Readers declare the versions they implement. An unsupported version is an unsupported-input result, not proof that the archive is invalid.

Patch releases cannot change archive validity or the manifest version. Compatible optional additions may appear in a later 1.x revision. New required fields, incompatible semantics, and additions to closed base value sets require a new major schema version. Unknown optional fields do not change the meaning of known base fields.

## Schema Locations

The 1.0 schema lives at [schema/1.0/oaa-manifest.schema.json](../schema/1.0/oaa-manifest.schema.json). Its `$id` identifies the immutable `v1.0.0` publication URL. Local validation uses the checked-in file without network access.

The historical [0.1 schema](../schema/oaa-manifest.schema.json) and [0.1 requirements](../requirements/oaa-0.1.yaml) remain unchanged. Release `v0.1.2` preserves the complete earlier contract. The reference validator in this checkout implements 1.0 only; 0.1 is reported as unsupported.

Supporting or migrating 0.1 is a separate, optional capability. Changing the version string alone is not a migration. See the [migration checklist](migration-0.1-to-1.0.md).
