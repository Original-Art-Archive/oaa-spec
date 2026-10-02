<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Original Art Archive (OAA) Format 1.0

## Status

This document defines specification release 1.0.0 of the Original Art Archive (OAA) Format. Manifests use `schema_version: "1.0"`.

The words MUST, MUST NOT, SHOULD, SHOULD NOT, MAY, and OPTIONAL are used to indicate requirement levels.

## Scope

OAA defines a platform-neutral archive format for transferring original art collection folders between collection platforms.

The base model is intentionally simple: an `.oaa` file is a ZIP-compatible archive of a portable original art collection directory.

This specification defines a portable collection folder model:

- A collection folder contains a collection manifest.
- Gallery folders contain gallery manifests, conventionally under `galleries/`.
- Artwork folders contain artwork manifests and associated files, conventionally under `artworks/`.
- Gallery manifests reference artworks by archive-local artwork ID, allowing one artwork to appear in multiple galleries without duplicating the artwork record.

This specification defines the archive folder layout and dot-manifest structure.

## Terminology

- **OAA archive**: A `.oaa` archive container that follows this specification.
- **Media type**: A string that identifies the archive format. The OAA media type is `application/vnd.original-art-archive+zip`.
- **Collection**: The top-level original art collection represented by the archive.
- **Gallery**: A subgroup within a collection.
- **Artwork**: A collection record that may belong to zero or more galleries and may have associated files.
- **Manifest**: A human-readable metadata file inside the archive.
- **Base field**: A field whose meaning is defined by OAA and can be understood across implementations.
- **Closed base value set**: A set of valid values for an OAA base field. Values outside the defined set are invalid.
- **Constrained string**: A string field whose value is validated by syntax or structure rather than by a fixed value list, such as an ID, path, URL, media type, or provider identifier.
- **Display-oriented string**: A human-readable string that readers should preserve and display as text, but must not treat as a controlled machine value.
- **Provider-specific value**: A value whose meaning is specific to an external provider, application, or workflow. Provider-specific values belong in extension blocks unless they can be mapped to valid OAA base values.
- **External link**: A provider-local identifier and URL assigned by an external service such as ComicArtFans, SNIKT.com, Raremarq, or the Library of Congress.
- **Extension container**: The JSON member named `extensions`; its value contains namespaced extension blocks.
- **Extension block**: A JSON object stored as a property value inside an extension container, containing implementation-specific or provider-specific data not defined as OAA base data.
- **Reader**: Software that reads OAA archive contents.
- **Writer**: Software that creates OAA archive contents.
- **Extension block field**: A field inside an extension block.

## Archive Container

An OAA archive distributed as a filesystem file SHOULD use the `.oaa` file extension.

Readers SHOULD NOT rely on the filename extension alone to identify OAA archives.

Readers MAY accept a structurally valid OAA archive even when the filename extension is missing or incorrect.

An OAA archive MUST be a ZIP-compatible archive container.

The OAA media type identifier is:

```text
application/vnd.original-art-archive+zip
```

Use of this identifier does not establish IANA registration. Registration is a tracked administrative follow-up, not a release prerequisite. Release documentation MUST state the actual registration status; this release makes no claim of registration.

OAA archive entries MUST NOT be encrypted.

Readers MUST reject encrypted archive entries.

Archive entries MUST use only the Store and Deflate ZIP compression methods.

Structural Readers MUST support Store, Deflate, and ZIP64 containers within declared resource limits.

An OAA archive SHOULD contain the contents of one collection folder.

An OAA archive does not require `META-INF/container.xml`. The root `.oacollection` manifest is the canonical collection manifest.

Archive entry names MUST use forward slash `/` separators.

Archive entry names MUST be UTF-8.

Archive entry names MUST NOT begin with `/`.

Archive entry names MUST NOT contain backslash `\` separators.

Archive entry names MUST NOT contain empty path segments, `.` segments, or `..` segments, except that the empty final segment of an explicit directory entry is allowed.

Explicit directory entries MAY be present. Readers MUST ignore explicit directory entries when interpreting archive structure. Writers SHOULD NOT include explicit directory entries. Readers MUST NOT require explicit directory entries. Directory structure is inferred from file entry paths.

Readers MUST treat path comparisons as case-sensitive inside the archive.

Archive path comparisons are byte-for-byte after UTF-8 decoding unless otherwise specified.

Writers SHOULD emit archive paths in Unicode Normalization Form C (NFC).

Readers MUST NOT normalize paths before safety validation.

A valid OAA archive MUST NOT contain duplicate archive entries with the same path. Readers MUST reject archives that contain duplicate archive entries with the same path.

Archive entries MUST be regular files or directories. Symbolic links and special filesystem entries MUST NOT be present. A file path MUST NOT also be a directory path or an ancestor of another entry. Readers MUST reject these conflicts and unsafe entry types.

## Required Files

An OAA archive MUST contain a `mimetype` file at the archive root.

The `mimetype` file MUST contain exactly this ASCII string, with no leading or trailing whitespace:

```text
application/vnd.original-art-archive+zip
```

Writers SHOULD place `mimetype` as the first ZIP entry and SHOULD store it without compression so archive type detection can be performed without inspecting the full package.

An OAA archive MUST contain one collection manifest at the archive root:

```text
.oacollection
```

Each gallery directory referenced by the collection manifest MUST contain one gallery manifest:

```text
.oagallery
```

Each artwork directory referenced by the collection manifest MUST contain one artwork manifest:

```text
.oaartwork
```

An artwork directory MAY contain associated image files, documents, video files, and other artwork-related files.

## Directory Layout

The recommended directory layout is:

```text
mimetype
.oacollection
galleries/
  <Gallery ID or slug>/
    .oagallery
artworks/
  <Artwork ID or slug>/
    .oaartwork
    <associated files>
```

Example:

```text
mimetype
.oacollection
galleries/
  AP/
    .oagallery
  For Sale/
    .oagallery
artworks/
  OAA-00044/
    .oaartwork
    hit-comics-5-cover.png
```

An **archive-relative path** is resolved from the root of the OAA archive.

An **artwork-relative path** is resolved from the directory containing the relevant `.oaartwork` manifest.

The collection manifest is authoritative for locating gallery and artwork manifests inside the archive. The `galleries/` and `artworks/` names are recommendations, not validity requirements.

Readers MUST NOT infer additional records by scanning unreferenced entries. Safe unreferenced extra content MAY exist; every entry MUST satisfy archive safety rules. Preservation of all extras is not a core conformance guarantee.

Empty collection registries, empty galleries, metadata-only artworks, and artworks without gallery membership are valid when their required structures are present. Readers MUST NOT invent gallery membership that is absent from the manifests.

Gallery manifests describe gallery membership. They MUST reference artworks by archive-local artwork ID and MUST NOT duplicate mutable artwork metadata such as title, provider IDs, artist credits, media, private metadata, or file lists.

One physical artwork directory SHOULD exist for each artwork record. Multiple galleries MAY reference the same artwork ID.

Within an artwork manifest, each `files[].relative_path` value is authoritative for locating that file and is resolved relative to the directory containing the `.oaartwork` manifest.

Readers MUST NOT infer artwork-associated files by scanning an artwork directory as a substitute for the `files[]` entries in the `.oaartwork` manifest.

Paths used to locate files within the archive MUST be relative paths within the archive.

Archive entry paths MUST NOT be absolute paths.

Archive entry paths MUST NOT contain NUL characters or drive-qualified forms, including drive-relative paths such as `C:filename`.

Archive entry paths MUST NOT contain parent-directory traversal segments such as `../`.

## Manifest Files

OAA manifests are human-readable JSON metadata files.

The manifest files are:

- `.oacollection`: collection manifest
- `.oagallery`: gallery manifest
- `.oaartwork`: artwork manifest

Manifest files MUST be encoded as UTF-8 JSON.

Manifest files MUST contain a JSON object at the top level.

Manifest JSON objects MUST NOT contain duplicate member names. Readers MUST reject duplicate member names and non-JSON numeric tokens, including `NaN` and `Infinity`.

Readers MUST NOT require a byte order mark.

Writers SHOULD NOT emit a byte order mark.

Manifest files SHOULD be written with stable field ordering and two-space indentation when practical.

Manifest files MUST include `schema_version`.

For 1.0, `schema_version` MUST be the string `"1.0"` in `.oacollection`, `.oagallery`, and `.oaartwork` manifests.

The normative JSON Schema for OAA 1.0 dot-manifest JSON structure is [schema/1.0/oaa-manifest.schema.json](schema/1.0/oaa-manifest.schema.json). Its immutable versioned identifier is `https://raw.githubusercontent.com/Original-Art-Archive/oaa-spec/v1.0.0/schema/1.0/oaa-manifest.schema.json`. Select the `collectionManifest`, `galleryManifest`, or `artworkManifest` definition according to the manifest path. The root schema accepts any manifest kind but cannot determine the kind from JSON alone.

The JSON Schema is normative for manifest-local field presence, JSON types, and constraints expressible within a single manifest document. This specification remains normative for archive layout, ZIP container requirements, cross-manifest references, embedded file existence, security, privacy, and reader/writer behavior.

All OAA manifest types in one archive MUST use the same `schema_version`.

Readers MUST explicitly recognize each supported schema version. Readers MUST NOT infer compatibility by ordering version strings or assume support for all future 1.x versions from support for `"1.0"`. An unsupported version MUST be reported as unsupported, not assumed invalid under its own contract.

Readers MUST ignore unrecognized optional fields and extension blocks when interpreting a manifest.

Writers SHOULD preserve unrecognized optional fields and extension blocks when rewriting a manifest, when practical.

## Required String Values

Required string fields MUST NOT be empty unless this specification explicitly defines an empty-string sentinel for that field.

Required names, titles, and identifiers MUST NOT contain only whitespace.

Writers SHOULD omit optional fields whose values are unavailable. `null` is valid only where the field definition explicitly permits it.

Readers SHOULD preserve string values as written.

Readers SHOULD NOT trim string values except when checking whether a required string is empty or whitespace-only.

Readers MUST NOT trim path fields before resolving them. Path fields MUST be validated exactly as written.

## Base Values and Extension Values

OAA base fields are intended to provide portable meaning across implementations.

A base field that defines a closed base value set MUST use one of the values defined by this specification. Unknown values in closed base value sets are invalid.

Closed base value sets are defined in the object field definitions where those fields are declared.

Provider-specific, application-specific, or otherwise non-portable values MUST NOT be written into closed base fields. Writers SHOULD preserve those values in extension blocks.

If a writer can map provider data to a valid OAA base value, it SHOULD write the mapped base value.

If a writer cannot map provider data to a valid OAA base value, it SHOULD omit the optional base field or use an OAA-defined fallback value, if one exists. The provider-specific value MAY be preserved in an extension block.

Extension blocks MAY preserve provider-native values, raw imported values, and implementation-specific values, but extension blocks MUST NOT override the meaning of OAA base fields.

Some base string fields are display-oriented. Display-oriented fields are portable as text only. Readers SHOULD preserve and display them, but MUST NOT infer controlled machine semantics from them.

## Manifest Structures

This section defines the three dot-manifest structures. Nested object types referenced by these manifests are defined in [Object Type Definitions](#object-type-definitions).

### Collection Manifest

The root `.oacollection` manifest describes the collection represented by the archive.

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `schema_version` | string | Yes | OAA manifest schema version. For 1.0, MUST be `"1.0"`. |
| `id` | string | Yes | Archive-local collection identifier. |
| `name` | string | Yes | Human-readable collection name. |
| `external_links` | array | No | External link objects for the collection. |
| `galleries` | array | Yes | Collection gallery reference objects contained in this collection. |
| `artworks` | array | Yes | Collection artwork reference objects contained in this collection. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

When present, each `external_links[]` entry MUST be an external link object.

Each `galleries[]` entry MUST be a collection gallery reference object.

Each `artworks[]` entry MUST be a collection artwork reference object.

Collection `galleries[].id` values MUST be unique within the collection manifest.

Collection `galleries[].path` values MUST be unique within the collection manifest.

Collection `galleries[].path` values MUST be archive-relative paths.

The `id` field inside a referenced `.oagallery` manifest MUST match the corresponding collection `galleries[].id` value.

The order of `galleries[]` in the collection manifest SHOULD be treated as collection gallery display order.

Collection `artworks[].id` values MUST be unique within the collection manifest.

Collection `artworks[].path` values MUST be unique within the collection manifest.

Collection `artworks[].path` values MUST be archive-relative paths.

The `id` field inside a referenced `.oaartwork` manifest MUST match the corresponding collection `artworks[].id` value.

The order of `artworks[]` in the collection manifest is registry order. Readers MAY use this order as fallback artwork display order when no gallery context is available.

### Gallery Manifest

Each `.oagallery` manifest describes one gallery within a collection.

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `schema_version` | string | Yes | OAA manifest schema version. For 1.0, MUST be `"1.0"`. |
| `id` | string | Yes | Archive-local gallery identifier. |
| `name` | string | Yes | Human-readable gallery name. |
| `external_links` | array | No | External link objects for the gallery. |
| `artworks` | array | Yes | Gallery artwork reference objects in display order. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

When present, each `external_links[]` entry MUST be an external link object.

Each `artworks[]` entry MUST be a gallery artwork reference object.

The `id` value in each gallery artwork reference MUST match an artwork reference in the root `.oacollection` manifest.

Within one `.oagallery` manifest, `artworks[].id` values MUST be unique.

The order of `artworks[]` in a gallery manifest SHOULD be treated as that gallery's artwork display order.

Gallery artwork references MAY include extension block fields for gallery-specific ordering or display hints, but they SHOULD NOT duplicate mutable artwork metadata. The artwork manifest remains the canonical home for artwork metadata and file references.

### Artwork Manifest

Each `.oaartwork` manifest describes one artwork and its associated files.

An artwork MAY appear in multiple galleries through gallery artwork references. The `.oaartwork` manifest is a single shared artwork record, not a gallery-specific copy.

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `schema_version` | string | Yes | OAA manifest schema version. For 1.0, MUST be `"1.0"`. |
| `id` | string | Yes | Archive-local artwork identifier. |
| `title` | string | Yes | Human-readable artwork title. |
| `external_links` | array | No | External link objects for the artwork. |
| `public_metadata` | object | No | Public or publishable artwork metadata. |
| `private_metadata` | object | No | Private collection-management metadata. |
| `files` | array | Yes | Source, derivative, and supporting artwork files in display order. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

When present, each `external_links[]` entry MUST be an external link object.

When present, the `public_metadata` field MUST be a public artwork metadata object.

When present, the `private_metadata` field MUST be a private artwork metadata object.

Each `files[]` entry MUST be an artwork file object.

## Object Type Definitions

This section defines the nested object types referenced by the manifest structures above.

### External Link Object

External link objects describe an OAA object's linkage to an external system. They do not describe the provider itself.

External link objects use the following fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `provider` | string | Yes | Provider identifier matching the OAA provider identifier grammar. |
| `id` | string | Yes | Provider-local object identifier. MUST NOT be empty. |
| `url` | string | Yes | Canonical provider URL for the linked external object. MAY be an empty string when no stable URL is known. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

External link objects SHOULD use this shape:

```json
{
  "provider": "com.example-provider",
  "id": "provider-local-id",
  "url": "https://example.com/object/provider-local-id",
  "extensions": {}
}
```

The `provider` value SHOULD be a reverse-DNS-style identifier controlled by the provider or writer, such as `com.comicartfans`, `com.snikt`, `com.raremarq`, `gov.loc`, or `app.oa-curator`.

Provider identifiers MUST contain only lowercase ASCII letters, digits, hyphen, underscore, and period.

Provider identifiers MUST NOT begin or end with a period.

Provider identifiers MUST NOT contain consecutive periods.

The `id` value is the provider-local object identifier. It MUST NOT be empty. For example, CAF piece IDs, SNIKT image IDs, Raremarq slugs, and Library of Congress item IDs all map to `external_links[].id`.

If a provider has no separate stable ID but has a stable object URL, writers MAY use the URL string, or a stable source key derived from that URL, as `external_links[].id`.

The `url` value is the canonical provider URL for the linked external object. It MAY be an empty string when no stable URL is known.

When `url` is non-empty, it MUST be an absolute URL: a syntactically valid absolute URI with a scheme and scheme-specific value. Relative references, whitespace, invalid percent escapes, and local-file references are invalid. HTTP and HTTPS URLs MUST include a host.

The `url` value MUST NOT be a `file://` URL or local filesystem path.

Readers MUST NOT automatically fetch or execute a URL merely because it appears in a manifest.

Readers SHOULD NOT display empty `url` values as clickable links.

Readers MAY display external links with empty `url` values using their `provider` and `id` fields.

Writers SHOULD include both `id` and `url` when both are known.

Readers MUST treat an absent `external_links` field the same as an empty array.

Writers MAY omit `external_links` when the array would be empty.

Provider examples in this specification are non-normative recommended shapes. Providers and applications MAY define official addenda that document their own provider keys, identity fields, and extension block fields.

Readers MUST NOT reject an external link only because its `provider` value is unknown.

Readers SHOULD preserve unknown external links when rewriting manifests. Repeated associations MAY produce a warning but MUST NOT make an archive invalid. Readers MUST NOT merge distinct artworks solely because they share an external link.

Readers MAY display unknown external links generically using their `provider`, `id`, and `url` fields.

All identifiers and URLs in this specification's examples are illustrative and non-normative.

### Collection Gallery Reference Object

Collection gallery reference objects use the following fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Archive-local gallery identifier. MUST match the referenced `.oagallery` manifest's `id`. |
| `name` | string | No | Human-readable gallery display name. Recommended for display. |
| `path` | string | Yes | Archive-relative path to the referenced `.oagallery` manifest. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

Collection gallery references SHOULD use this shape:

```json
{
  "id": "gallery-stable-id",
  "name": "AP",
  "path": "galleries/AP/.oagallery",
  "extensions": {}
}
```

### Collection Artwork Reference Object

Collection artwork reference objects use the following fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Archive-local artwork identifier. MUST match the referenced `.oaartwork` manifest's `id`. |
| `path` | string | Yes | Archive-relative path to the referenced `.oaartwork` manifest. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

Collection artwork references SHOULD use this shape:

```json
{
  "id": "OAA-00044",
  "path": "artworks/OAA-00044/.oaartwork",
  "extensions": {}
}
```

### Gallery Artwork Reference Object

Gallery artwork reference objects use the following fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Archive-local artwork identifier. MUST match an artwork reference in the root `.oacollection` manifest. |
| `extensions` | object | No | Extension container for gallery-specific extension blocks. |

Gallery artwork references SHOULD use this shape:

```json
{
  "id": "OAA-00044",
  "extensions": {}
}
```

### Public Artwork Metadata Object

The `public_metadata` object MAY contain:

All fields inside `public_metadata` are optional unless this specification explicitly says otherwise.

Writers MAY omit `public_metadata` when no public metadata fields are known or applicable.

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `description` | string or null | No | Public description or notes. |
| `for_sale_status` | string or null | No | Display-oriented availability or sale-status text. Readers SHOULD preserve and display this value but MUST NOT treat it as a controlled OAA value. |
| `media` | string or null | No | Display-oriented media description. Readers SHOULD preserve and display this value but MUST NOT treat it as a controlled OAA value. |
| `artwork_type` | string or null | No | Display-oriented artwork type description. Readers SHOULD preserve and display this value but MUST NOT treat it as a controlled OAA value. |
| `publication_status` | string or null | No | Optional closed base value describing publication status. Valid values are `published_art` and `unpublished_art`. |
| `is_public` | boolean or null | No | Public visibility flag when known. |
| `artist_credits` | array | No | Artist credit objects in display order. |
| `extensions` | object | No | Extension container for public metadata extension blocks. |

When present, each `artist_credits[]` entry MUST be an artist credit object.

Writers MAY omit `artist_credits` when no artist credits are known or applicable.

When present and not null, `publication_status` MUST be one of:

| Value | Meaning |
| --- | --- |
| `published_art` | The artwork was created for, used in, or associated with publication. |
| `unpublished_art` | The artwork was not created for, used in, or associated with publication, or is known as unpublished artwork. |

Unknown `publication_status` values are invalid when `publication_status` is present and not null.

Provider-specific publication states, such as CAF `CAF Member Art`, belong in `public_metadata.extensions`. If a provider-specific publication state cannot be mapped to `published_art` or `unpublished_art`, writers SHOULD omit the base `public_metadata.publication_status` field and preserve the provider-specific value in an extension block.

### Artist Credit Object

Artist credit objects use the following fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `display_name` | string or null | No | Display-ready artist name when known. |
| `first_name` | string or null | No | Artist given name or first-name component when known. |
| `last_name` | string or null | No | Artist family name or last-name component when known. |
| `role` | string or null | No | Display-oriented creator role. Readers SHOULD preserve and display this value but MUST NOT treat it as a controlled OAA value. |
| `extensions` | object | No | Extension container for artist-credit extension blocks. |

Artist credit objects MUST include at least one nonempty string value in `display_name`, `first_name`, `last_name`, or `role`.

Artist credits SHOULD use this shape:

```json
{
  "display_name": "Example Artist",
  "first_name": "Example",
  "last_name": "Artist",
  "role": "Penciller",
  "extensions": {
    "com.example-provider": {
      "role_id": "example-role-id"
    }
  }
}
```

### Private Artwork Metadata Object

The `private_metadata` object MAY contain:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `purchase_price` | string or null | No | Private purchase price. |
| `estimated_value` | string or null | No | Private estimated value. |
| `purchase_date` | string or null | No | Private acquisition date. When present and non-null, use a real calendar date in `YYYY-MM-DD` form. |
| `provenance` | string or null | No | Private provenance and acquisition history. |
| `personal_notes` | string or null | No | Private collector notes. |
| `extensions` | object | No | Extension container for private metadata extension blocks. |

Private metadata, including its extension blocks, MUST NOT be treated as public publishing metadata by default.

`is_public` expresses record visibility intent, not blanket publication authorization. Readers MUST NOT publish private metadata, unknown extension content, receipts, or supporting files merely because a record is public. Publication of private or otherwise unclassified content requires explicit user authorization appropriate to that content.

Writers SHOULD provide an option to omit `private_metadata` when creating archives intended for transfer to third-party platforms.

Readers MUST NOT publish, upload, or display `private_metadata` in public contexts unless the user explicitly chooses to do so.

### Artwork File Object

The `files` field MUST be an array. The array MAY be empty for metadata-only artwork records, but each entry in the array MUST be a JSON object.

Artwork file entries use the following fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Stable file identifier unique within the artwork manifest. Other file entries and extension block fields MAY reference this value. |
| `relative_path` | string | Yes | Path to the file inside the archive, relative to the artwork directory. |
| `file_kind` | string | Yes | Closed base value describing the file role. Valid values are `raw`, `derivative`, and `supporting`. |
| `file_name` | string | No | Display file name. Readers MAY derive this from the last segment of `relative_path` when omitted. |
| `size_bytes` | integer or null | No | File size in bytes when known. |
| `width` | integer or null | No | Pixel width for image files when known. |
| `height` | integer or null | No | Pixel height for image files when known. |
| `dpi_x` | number or null | No | Horizontal DPI or pixels-per-inch value when known. |
| `dpi_y` | number or null | No | Vertical DPI or pixels-per-inch value when known. |
| `format` | string or null | No | Display-oriented file format label. Readers MUST NOT rely on this field for security-sensitive processing. Use `media_type`, file extension, and/or detected file type as appropriate. |
| `media_type` | string or null | No | MIME/media type for the file, such as `image/jpeg`, `image/png`, `application/pdf`, or `video/mp4`, when known. |
| `is_primary` | boolean or null | No | Whether this file is the preferred representative file for the artwork. Omitted or null means unknown or false. |
| `image_role` | string or null | No | Optional closed base value describing the image role. Valid values are `raw_scan`, `raw_photo`, `corrected_scan`, `detail`, `verso`, and `reference`. |
| `external_links` | array | No | External link objects for the file. |
| `extensions` | object | No | Extension container for namespaced extension blocks. |

When present and non-null, `size_bytes` MUST be a nonnegative integer equal to the embedded file's uncompressed byte length. Pixel `width` and `height` MUST be positive integers; `dpi_x` and `dpi_y` MUST be finite positive numbers. These constraints validate supplied metadata and do not require decoding media to verify pixel or DPI claims. Allowed nulls remain valid.

`file_kind` is required and MUST be one of:

| Value | Meaning |
| --- | --- |
| `raw` | A source or primary capture file, such as an original scan or photo. |
| `derivative` | A generated, transformed, resized, corrected, exported, or otherwise derived file. |
| `supporting` | A supporting file associated with the artwork, such as provenance, receipts, reference material, documentation, or other non-primary supporting content. |

Unknown `file_kind` values are invalid.

Provider-specific file kinds, upload slots, workflow states, or finer-grained file roles MUST be stored in extension blocks.

`image_role` is optional. When present and not null, it MUST be one of:

| Value | Meaning |
| --- | --- |
| `raw_scan` | An uncorrected or source scan. |
| `raw_photo` | An uncorrected or source photograph. |
| `corrected_scan` | A corrected, cleaned, cropped, color-adjusted, or otherwise prepared scan. |
| `detail` | A detail image showing a portion of the artwork. |
| `verso` | An image of the back side of the artwork. |
| `reference` | A reference image associated with the artwork. |

Unknown `image_role` values are invalid when `image_role` is present and not null.

If no OAA base value applies, writers SHOULD omit `image_role` or set it to null. Provider-specific or more precise image roles MUST be stored in extension blocks.

Artwork file entries SHOULD use this shape:

```json
{
  "id": "file-1",
  "file_name": "example-001.jpg",
  "relative_path": "example-001.jpg",
  "file_kind": "raw",
  "size_bytes": 123456,
  "width": 1200,
  "height": 800,
  "dpi_x": 96,
  "dpi_y": 96,
  "format": "JPEG",
  "media_type": "image/jpeg",
  "is_primary": true,
  "image_role": "raw_scan",
  "external_links": [
    {
      "provider": "com.example-provider",
      "id": "example-file-id",
      "url": "https://example.com/images/example.jpg"
    }
  ],
  "extensions": {
    "app.example-reader": {
      "source_kind": "imported"
    }
  }
}
```

Derivative files use the same object shape with derivative-specific fields:

```json
{
  "id": "file-2",
  "file_name": "example-001-web.png",
  "relative_path": "example-001-web.png",
  "file_kind": "derivative",
  "format": "PNG",
  "media_type": "image/png",
  "width": 571,
  "height": 800,
  "external_links": [],
  "extensions": {
    "app.example-reader": {
      "source_file_id": "file-1",
      "derivative_type": "web_export"
    },
    "com.example-provider": {
      "upload_profile": "example-web-profile"
    }
  }
}
```

Every file entry in `files[]` MUST identify a file embedded in the OAA archive.

`relative_path` MUST be an artwork-relative path and MUST resolve to a file within the artwork directory.

`relative_path` MUST NOT be absolute and MUST NOT contain parent-directory traversal segments such as `../`.

To resolve `files[].relative_path`, readers MUST join it to the archive-relative directory containing the relevant `.oaartwork` manifest, then validate the resulting archive-relative path using the archive path rules.

External source URLs or provider-hosted file references MAY be preserved in `external_links` or extension blocks, but they are not substitutes for `files[].relative_path`.

When `file_name` is present, it SHOULD match the last path segment of `relative_path` unless the writer is preserving a provider display name that differs from the archive path.

At most one file entry per artwork SHOULD have `is_primary: true`.

If multiple files have `is_primary: true`, readers SHOULD use the first such file in `files[]` order.

If no file entry has `is_primary: true`, readers SHOULD choose the first `raw` file when practical, otherwise the first file entry.

`media_type` SHOULD use a registered media type when known.

Readers MUST NOT trust `format`, `media_type`, or file extension for security-sensitive processing.

Readers MAY warn when `format`, `media_type`, file extension, or detected file type appear inconsistent.

Implementation-specific file workflow data, such as source-link status, derivative generation workflow, or local source-file relationships, SHOULD be stored in namespaced extension blocks.

### Extension Container and Extension Block

Every complex base OAA object MAY contain an extension container. Inside an extension block, keys are provider data rather than base fields.

An extension container is serialized as the JSON member named `extensions`.

Each property of an extension container is an extension block.

Extension containers MAY appear on complex OAA objects.

Extension block values MUST be JSON objects.

Extension blocks MAY contain ordinary nested JSON, including a key named `extensions`; that key has no OAA extension-container meaning inside provider data.

Readers MUST treat an absent `extensions` field the same as an empty extension container.

Writers MAY omit `extensions` when the extension container would be empty.

Writers MUST use reverse-DNS-style namespace names for newly created extension blocks, controlled by the writer or provider. Namespace ownership is a writer responsibility, not an online lookup or reader rejection criterion. Non-normative examples include:

- `com.comicartfans`
- `com.snikt`
- `com.raremarq`
- `app.oa-curator`

## Data Model and Extensibility

OAA separates shared meaning from provider-specific meaning:

- Base fields define portable OAA concepts.
- External link objects store IDs and URLs assigned by external providers or systems.
- Extension blocks store implementation-specific or provider-specific data that OAA does not standardize.

If a value has a general OAA meaning, writers SHOULD put the normalized value in a base field. If a provider also has its own special representation of that value, writers SHOULD preserve that representation in that provider's extension block.

For example, an artwork can store a base media label and a provider-specific controlled media code:

```json
{
  "public_metadata": {
    "media": "Paint - Watercolor",
    "extensions": {
      "com.example-provider": {
        "media_code": "example-watercolor-code"
      }
    }
  }
}
```

Provider-specific values that are alternatives to optional OAA fields SHOULD live in the extension container of the complex object they modify.

Extension blocks MUST NOT change the meaning of base OAA fields.

An extension field MAY have the same name as a present base field. Base fields MUST remain authoritative for OAA interpretation; base-field value rules MUST NOT be imposed on unrelated keys inside provider extensions.

Writers MUST place newly created provider-specific or implementation-specific fields in namespaced extensions. Existing structured source data SHOULD be preserved there; flattening it into display text is not equivalent preservation.

Extension block fields MAY preserve provider-native names or raw imported values, but MUST NOT override the base field.

Extension blocks MUST NOT override `external_links[].url`. If an extension block needs to preserve additional URLs, it SHOULD use clearly named secondary fields such as `source_image_url`, `thumbnail_url`, `master_image_url`, or `alternate_urls`.

Readers MUST ignore unrecognized optional fields when interpreting a manifest.

Readers MUST ignore extension blocks they do not understand.

Writers SHOULD preserve extension blocks they do not understand when rewriting a manifest, when practical.

Provider-specific extension block examples are non-normative and live in [docs/provider-extensions.md](docs/provider-extensions.md).

## Artwork File Rules

Artwork-associated files SHOULD live in the same artwork directory as the `.oaartwork` manifest.

Artwork file references MUST resolve to files within the archive.

Artwork-associated files MAY be any file type relevant to the artwork record. Examples include original scans or photos, corrected images, PDF receipts, email thread archives, provenance documents, videos showing creation or authentication of the artwork, and other supporting files.

Actual portable file references MUST NOT be absolute local filesystem paths. Path-like prose or opaque extension text MAY produce an advisory privacy warning but MUST NOT be rejected solely on that heuristic.

Portable file references MUST use archive-relative or artwork-relative paths as defined by this specification.

OAA does not require readers to decode, render, or open JPEG, PNG, TIFF, PDF, video, or any other embedded data file type.

Readers MAY preserve embedded file metadata and bytes while marking the file type as unsupported by that reader.

## Identifiers

OAA IDs are archive-local opaque identifiers.

This specification does not require a particular ID format. Examples may use `OAA-00044` for readability, but writers MAY use any string that satisfies the ID rules and is unique within its stated record-type scope.

Readers MUST NOT infer provider, ownership, chronology, permanence, or global identity from an OAA ID.

OAA IDs are only guaranteed to be stable within a single archive instance. Readers SHOULD NOT assume the same artwork, gallery, or file will have the same OAA ID across separate exports, revisions, or regenerated archives.

Gallery IDs MUST be unique among galleries; artwork IDs MUST be unique among artworks. The single collection ID and identifiers of different record types MAY reuse the same string.

File IDs MUST be unique within their containing artwork manifest.

Providers MAY use their own identifiers as OAA IDs when writing an archive. Even when doing so, writers SHOULD also preserve provider-assigned IDs in the relevant `external_links` block so readers can identify the source system and avoid parsing meaning from the OAA ID string.

Provider-assigned identifiers, such as CAF piece IDs, SNIKT image IDs, Raremarq slugs, or Library of Congress item IDs, SHOULD be represented in `external_links` when available.

## Dates and Times

When present and non-null, base date-only fields MUST be real calendar dates in `YYYY-MM-DD` form. These rules do not apply to unrelated extension keys.

Date-time base fields, where defined, MUST use RFC 3339 with an explicit timezone or offset. This rule does not impose date-time semantics on unrelated provider extension keys.

Acquisition and purchase dates MAY be date-only local dates.

## Base Value Classification

The following OAA base fields use closed base value sets:

- `public_metadata.publication_status`
- `files[].file_kind`
- `files[].image_role`

The following OAA base fields are display-oriented strings:

- `public_metadata.media`
- `public_metadata.artwork_type`
- `artist_credits[].role`
- `public_metadata.for_sale_status`
- `files[].format`

Readers SHOULD preserve and display display-oriented string fields, but MUST NOT treat them as controlled OAA machine values.

Provider identifiers, OAA IDs, paths, URLs, media types, and dates are constrained strings. They are validated by syntax or structure rather than by closed value sets.

Provider-specific or application-specific values that do not map to OAA base values belong in extension blocks.

## Validation

A valid OAA archive satisfies these checks:

- If distributed as a filesystem file, the archive should use the `.oaa` extension.
- The archive is ZIP-compatible.
- Archive entries are not encrypted.
- All entries use Store or Deflate compression; ZIP64 is allowed.
- Archive entry names use UTF-8 and forward slash separators.
- Archive entry names do not begin with `/`.
- Archive entry names do not contain empty, `.`, or `..` path segments, except for the empty final segment of explicit directory entries.
- Archive path comparisons are byte-for-byte after UTF-8 decoding.
- Writers should emit archive paths in Unicode Normalization Form C (NFC).
- Readers do not normalize paths before safety validation.
- Explicit directory entries, if present, are ignored when interpreting archive structure.
- The archive does not contain duplicate entries with the same path.
- The archive root contains `mimetype`.
- The `mimetype` file contains `application/vnd.original-art-archive+zip`.
- The archive root contains `.oacollection`.
- Referenced gallery folders contain `.oagallery`.
- Referenced artwork folders contain `.oaartwork`.
- Required manifest fields are present.
- Collection manifests include required `id` values.
- Collection gallery references include `id` and `path`.
- Collection artwork references include `id` and `path`.
- Collection gallery reference IDs are unique.
- Collection artwork reference IDs are unique.
- Collection gallery reference paths are unique.
- Collection artwork reference paths are unique.
- Referenced gallery manifest IDs match the corresponding collection gallery reference IDs.
- Referenced artwork manifest IDs match the corresponding collection artwork reference IDs.
- Gallery manifests include required `id` values.
- Gallery artwork references resolve to artwork IDs listed by the collection manifest.
- Gallery artwork reference IDs are unique within each gallery manifest.
- File entry IDs are unique within each artwork manifest.
- External link provider identifiers follow the provider identifier grammar.
- External link IDs are not empty.
- External link URLs are strings; empty URLs are allowed only as the explicit no-stable-URL sentinel.
- Actual file references are safe; path-like prose alone does not establish invalidity.
- Extension block values are JSON objects; nested provider JSON and same-named provider fields are allowed.
- Field values match their declared types.
- `public_metadata.publication_status`, when present and not null, is `published_art` or `unpublished_art`.
- `files[].file_kind` values are `raw`, `derivative`, or `supporting`.
- `files[].image_role`, when present and not null, is `raw_scan`, `raw_photo`, `corrected_scan`, `detail`, `verso`, or `reference`.
- Required string fields are not empty except where this specification explicitly allows an empty-string sentinel.
- Archive paths are relative.
- Archive paths do not contain parent-directory traversal.
- Referenced artwork files exist inside the archive when required.

Archive-validity `MUST` and `MUST NOT` violations make the archive invalid. A `SHOULD` violation alone produces an advisory warning. Readers MAY recover usable content from invalid input, but MUST identify invalidity and known skipped or lost content; recovery is optional, not a conformance prerequisite.

Readers MUST distinguish invalidity from unsupported versions, configured capacity limits, and destination-specific extraction limitations. A reader that stops before completing validation MUST NOT report full validation. Resource exhaustion alone is not proof of archive invalidity.

Recommended validation diagnostics:

| Condition | Severity | Reader behavior |
| --- | --- | --- |
| Missing `mimetype` | Fatal | Reject archive. |
| Wrong `mimetype` | Fatal | Reject archive. |
| Missing `.oacollection` | Fatal | Reject archive. |
| Unsafe path traversal | Fatal | Reject archive. |
| Duplicate archive entry path | Fatal | Reject archive. |
| Duplicate collection gallery or artwork ID | Fatal | Reject archive. |
| Duplicate collection gallery or artwork path | Fatal | Reject archive. |
| Duplicate gallery artwork membership ID | Fatal | Reject archive or affected gallery. |
| Referenced gallery or artwork manifest missing | Fatal or record error | Reject archive or reject the affected record. |
| Gallery reference to unknown artwork ID | Fatal or record error | Reject the affected gallery membership. |
| Missing embedded file for `files[]` entry | Invalid archive | Optional metadata recovery with explicit missing-file report. |
| Unknown or invalid `public_metadata.publication_status` | Fatal | Reject archive or affected artwork record. |
| Unknown or invalid `files[].file_kind` | Fatal | Reject archive or affected artwork record. |
| Unknown or invalid `files[].image_role` | Fatal | Reject archive or affected artwork record. |
| Unknown extension block | Info | Ignore for interpretation and preserve when practical. |
| Unknown external link provider | Info | Preserve and display generically when practical. |
| Unsupported embedded file type | Info | Preserve file metadata and bytes when practical; decoding/rendering is outside OAA conformance. |
| Multiple primary files | Warning | Use the first primary file in `files[]` order. |

## Reader Requirements

A Structural Reader MUST be able to locate the collection manifest.

A Structural Reader MUST validate the root `mimetype` file.

A Structural Reader MUST be able to locate gallery and artwork manifests from references in the collection manifest.

A Structural Reader MUST resolve gallery artwork membership references by artwork ID.

A Structural Reader MUST be able to identify files associated with each artwork directory.

A Structural Reader MUST resolve artwork-associated files through `files[].relative_path`.

A Structural Reader MUST reject unsafe archive entry paths.

A Structural Reader MUST ignore unrecognized optional fields when interpreting a manifest.

A Metadata Reader is also a Structural Reader.

A Metadata Reader MUST import or display OAA base metadata and external links.

Structural and Metadata Readers MAY support no rendering at all.

OAA reader conformance does not require decoding, rendering, previewing, or opening any embedded file format.

Implementations SHOULD preserve unknown optional fields, external links, and extension data when rewriting manifests. Supporting import and export or producing a Valid Archive MUST NOT alone be presented as proof of lossless preservation. Preservation claims MUST state their evidence, scope, and limitations.

Reader behavior beyond the conformance level claimed is outside this file-format specification.

## Writer Requirements

A conforming writer MUST produce valid `.oaa` archives according to this specification.

A conforming writer MUST write the required root `mimetype` file.

A conforming writer MUST use the required collection, gallery, and artwork manifest structure.

A conforming writer MUST reference artworks in gallery manifests by artwork ID.

A conforming writer MUST NOT create archive entries with absolute paths.

A conforming writer MUST NOT create archive entries with parent-directory traversal paths.

How a writer gathers source data before creating an archive is outside this file-format specification.

## Conformance Levels

### Valid Archive

A Valid Archive follows the container, layout, manifest, path, and field rules in this specification.

### Structural Reader

A Structural Reader can validate package structure, parse manifests, resolve galleries and artworks, and locate embedded files according to this specification.

### Metadata Reader

A Metadata Reader is also a Structural Reader.

A Metadata Reader can import or display OAA base metadata, external links, and raw unknown extension block data when practical.

### Conforming Writer

A Conforming Writer produces Valid Archives according to this specification.

## Compatibility

OAA is intended for interchange between independent original art collection platforms.

Specification release `1.0.0` uses manifest version `"1.0"`. Release tags use `MAJOR.MINOR.PATCH`. Patch releases MUST NOT change manifest version, add archive-validity requirements, or retroactively invalidate conforming archives.

Compatible optional fields MAY be added in later 1.x revisions without changing existing semantics. New required fields, incompatible semantic changes, and additions to closed value sets require a new major schema version.

Readers MUST ignore unrecognized optional fields. The existing base field set is frozen for 1.0. New shared metadata and the expanded lossless-preservation profile remain deferred.

OAA 0.1 archives are not 1.0 Valid Archives. Implementations MAY separately declare 0.1 compatibility or migration support; migration is not mandatory. The historical 0.1 contract remains available in release `v0.1.2`, the unchanged [0.1 schema](schema/oaa-manifest.schema.json), and [0.1 requirements](requirements/oaa-0.1.yaml).

## Security and Privacy Considerations

Readers MUST treat archive contents as untrusted input.

Readers MUST treat embedded artwork-associated files as untrusted content.

Archive extraction MUST NOT write outside the selected destination. Readers that extract MUST detect destination-specific collisions, including case and Unicode normalization collisions, before writing content and MUST NOT silently overwrite colliding entries. Readers MUST also prevent traversal through existing destination symlinks or other filesystem redirections. Archive identities remain case-sensitive; inability to represent safe distinct paths on a destination is not itself archive invalidity. Conformance does not require extraction onto every filesystem.

Actual archive and file references MUST be safe. Apparent local paths in prose or opaque extension data are privacy warnings, not automatic validity failures.

Manifest URL fields MUST be used for URLs only. They MUST NOT use `file://` URLs or local filesystem paths as portable file references.

Readers SHOULD avoid opening or rendering embedded files automatically. Opening or rendering embedded files is implementation-specific and at the reader's or user's risk.

Readers that open or render embedded files SHOULD use hardened rendering paths and should not grant embedded files access to local files, network resources, or application privileges without explicit user action.

Writers SHOULD avoid including credentials, browser cookies, or unrelated private files.

Readers MUST enforce declared resource limits on archive size, uncompressed size, entry count, manifest size, JSON nesting depth, and individual file size. Reads and decompression MUST be bounded, including for unreferenced content. Limits are implementation-configurable; OAA defines no universal capacity threshold. Declared and actual decompressed sizes must be checked without unbounded allocation. Exceeding limits MUST be reported as incomplete processing, not proof of invalidity.

## Examples

OAA 1.0 examples live in [examples/](examples/).

Complete minimal manifest examples are included below for convenience.

### Minimal `.oacollection`

```json
{
  "schema_version": "1.0",
  "id": "example-collection",
  "name": "Example Collection",
  "external_links": [],
  "galleries": [
    {
      "id": "example-gallery",
      "name": "Example Gallery",
      "path": "galleries/Example Gallery/.oagallery",
      "extensions": {}
    }
  ],
  "artworks": [
    {
      "id": "OAA-00001",
      "path": "artworks/OAA-00001/.oaartwork",
      "extensions": {}
    }
  ],
  "extensions": {}
}
```

### Minimal `.oagallery`

```json
{
  "schema_version": "1.0",
  "id": "example-gallery",
  "name": "Example Gallery",
  "external_links": [],
  "artworks": [
    {
      "id": "OAA-00001",
      "extensions": {}
    }
  ],
  "extensions": {}
}
```

### Minimal `.oaartwork`

```json
{
  "schema_version": "1.0",
  "id": "OAA-00001",
  "title": "Example Artwork",
  "external_links": [],
  "files": [],
  "extensions": {}
}
```

### Multi-Gallery Membership

The same artwork can appear in more than one gallery by listing the artwork once in the root `.oacollection` artwork registry, then referencing that artwork ID from multiple `.oagallery` manifests.

Example `.oacollection`:

```json
{
  "schema_version": "1.0",
  "id": "example-collection",
  "name": "Example Collection",
  "external_links": [],
  "galleries": [
    {
      "id": "gallery-ap",
      "name": "AP",
      "path": "galleries/AP/.oagallery",
      "extensions": {}
    },
    {
      "id": "gallery-for-sale",
      "name": "For Sale",
      "path": "galleries/For Sale/.oagallery",
      "extensions": {}
    }
  ],
  "artworks": [
    {
      "id": "OAA-00044",
      "path": "artworks/OAA-00044/.oaartwork",
      "extensions": {}
    }
  ],
  "extensions": {}
}
```

Example `galleries/AP/.oagallery`:

```json
{
  "schema_version": "1.0",
  "id": "gallery-ap",
  "name": "AP",
  "external_links": [],
  "artworks": [
    {
      "id": "OAA-00044",
      "extensions": {}
    }
  ],
  "extensions": {}
}
```

Example `galleries/For Sale/.oagallery`:

```json
{
  "schema_version": "1.0",
  "id": "gallery-for-sale",
  "name": "For Sale",
  "external_links": [],
  "artworks": [
    {
      "id": "OAA-00044",
      "extensions": {}
    }
  ],
  "extensions": {}
}
```

Example `artworks/OAA-00044/.oaartwork`:

```json
{
  "schema_version": "1.0",
  "id": "OAA-00044",
  "title": "Example Shared Artwork",
  "external_links": [],
  "files": [],
  "extensions": {}
}
```

## Changelog

See [CHANGELOG.md](CHANGELOG.md).
