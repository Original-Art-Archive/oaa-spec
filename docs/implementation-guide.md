<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Implementation Guide

This guide is non-normative. The normative format rules are in [../SPEC.md](../SPEC.md).

## Reading Archives

Software that reads an `.oaa` archive generally needs to:

- Confirm the archive is ZIP-compatible.
- Treat `mimetype` as the archive identity anchor and not rely on filename extension alone.
- Confirm the root `mimetype` file contains `application/vnd.original-art-archive+zip`.
- Reject encrypted archive entries.
- Support Store, Deflate, and ZIP64 within declared capacities; reject other compression methods.
- Read the collection manifest before processing child folders.
- Confirm each manifest uses an explicitly supported `schema_version`; report unsupported versions separately from invalid archives.
- Read gallery and artwork manifests from the paths listed in the collection manifest.
- Do not infer records from unreferenced files. Safe extras, empty registries, metadata-only artworks, and artworks without galleries are allowed.
- Resolve gallery artwork membership by artwork ID.
- Resolve collection `galleries[].path` and `artworks[].path` values as archive-relative paths.
- Use collection `galleries[]` order as gallery display order when practical.
- Treat collection `artworks[]` order as registry order, usable as fallback artwork display order when no gallery context is available.
- Reject duplicate collection gallery paths and duplicate collection artwork paths.
- Reject duplicate artwork membership IDs within one gallery manifest.
- Use gallery `artworks[]` order as artwork display order within that gallery.
- Resolve artwork-associated files from `files[].relative_path` values as artwork-relative paths, relative to the directory containing the `.oaartwork` manifest.
- Validate the joined artwork file path using the archive path rules before using it.
- Treat `files[]` entries as embedded archive files. Provider URLs can preserve source identity, but they are not substitutes for embedded file entries.
- Select representative artwork files by first `is_primary: true`, then first `raw` file, then first file entry.
- Preserve embedded file metadata and bytes when practical, even if the reader cannot decode or render the file.
- Use base fields for portable metadata.
- Treat display-oriented string fields as text, not as controlled machine values.
- Reject unknown values in closed base value sets: `public_metadata.publication_status`, `files[].file_kind`, and `files[].image_role`.
- Use external link objects to associate provider-local IDs and URLs.
- Display external links with empty URLs as metadata, not clickable links.
- Preserve and optionally display unknown external links generically.
- Ignore unknown extension blocks.
- Treat extension blocks as opaque JSON, including nested provider JSON and same-named fields; base fields remain authoritative.
- Preserve unknown external links and extension blocks when practical if the implementation later writes OAA archives.
- Validate all archive paths.
- Validate paths before Unicode normalization.
- Reject absolute paths.
- Reject parent-directory traversal.
- Warn about apparent local paths in prose or opaque extensions as a privacy concern; reject unsafe actual archive/file references.
- Reject special entries, symlinks, duplicate paths, and file/directory conflicts before reading payloads.
- Bound archive metadata, entry counts, every decompression/read, file and total sizes, manifest sizes, and JSON nesting. A capacity stop is incomplete processing, not proof of invalidity.
- Check declared embedded sizes against actual uncompressed bytes, numeric ranges, nonblank required strings, actual calendar dates, and absolute non-local URLs. Reject duplicate JSON members and non-JSON numeric tokens.
- Ignore explicit ZIP directory entries when interpreting archive structure.
- Treat embedded artwork-associated files as untrusted. Avoid opening or rendering embedded files automatically.
- Treat media decoding and rendering as implementation-specific behavior outside OAA base conformance.

Reader safety and privacy obligations still apply after parsing. Do not automatically fetch URLs, execute or render embedded files, or publish unknown extension data and supporting attachments. `is_public` is not blanket disclosure permission. Keep `private_metadata`, including its extensions, out of public output unless separately authorized.

If extracting, detect destination case/Unicode collisions and existing symlinks or other filesystem redirections before writing. Do not overwrite colliding entries or escape the destination. A safe archive may be unrepresentable on a particular filesystem without being invalid. A non-extracting reader can avoid this entire write surface.

## Writing Archives

Software that writes an `.oaa` archive generally needs to create a valid archive layout:

- Root `mimetype`
- Root `.oacollection`
- Referenced gallery folders containing `.oagallery` (conventionally under `galleries/`)
- Referenced artwork folders containing `.oaartwork` (conventionally under `artworks/`)
- Associated files in artwork folders
- Gallery artwork membership references by artwork ID
- Artwork file references through `files[].relative_path`
- Embedded files for each `files[]` entry
- Base fields for shared metadata.
- Valid OAA values for closed base value sets, including `public_metadata.publication_status`, `files[].file_kind`, and `files[].image_role`.
- Display-oriented strings for human-readable values that OAA does not interpret as controlled machine values.
- External link objects for external service IDs and URLs.
- Extension blocks for data that is not standardized by OAA.
- Provider-specific values mapped to valid OAA base values where possible, with provider-native details preserved in extension blocks.
- No encrypted archive entries.
- Store or Deflate compression for archive entries.
- No unsafe archive entry paths

For a collection folder that already follows the OAA layout, creating an `.oaa` archive can be as simple as packaging the collection folder contents into a ZIP-compatible container, adding the root `mimetype` entry, and using the `.oaa` extension.

Validate the emitted archive, not only the input folder. Preserve structured source data in extensions when the base model cannot represent it; flattening it into title/notes is not lossless preservation. State the scope and evidence for any preservation claim. See [conformance](conformance.md) and the [release verification scope](release-1.0.0.md).
