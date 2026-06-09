<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Versioning

OAA uses two related version forms:

- **Manifest schema versions** are compatibility identifiers stored in manifests as `schema_version`. These use `MAJOR.MINOR` strings, such as `"0.1"`.
- **Specification release tags** may use SemVer-style `MAJOR.MINOR.PATCH` tags, such as `v0.1.0` or `v0.1.1`.

The manifest `schema_version` does not include a patch component. A patch release clarifies the specification without changing which archives are valid, so it does not change manifest `schema_version`.

MAJOR manifest schema versions may introduce incompatible changes.

MINOR manifest schema versions may add backward-compatible fields or behaviors.

New optional fields may be added in a minor manifest schema version.

New required fields must not be added except in a major manifest schema version.

OAA 1.x readers must ignore unknown optional fields.

## Draft Versions

Versions before 1.0 are drafts and may change incompatibly while the format is being developed.

For the 0.1 draft, each manifest uses the string value:

```json
{
  "schema_version": "0.1"
}
```

Readers must reject manifests with an unsupported higher required schema version unless they explicitly support that version.

Readers may accept lower schema versions only when they implement compatibility handling for those versions.
