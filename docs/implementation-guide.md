<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Implementation Guide

This guide is non-normative. The normative format rules are in [../SPEC.md](../SPEC.md).

## Reading Archives

Software that reads an `.oaa` archive generally needs to:

- Confirm the archive is ZIP-compatible.
- Treat `mimetype` as the archive identity anchor and not rely on filename extension alone.
- Confirm the root `mimetype` file contains `application/vnd.original-art-archive+zip`.
- Reject encrypted archive entries.
- Support Store and Deflate ZIP compression methods.
- Read the collection manifest before processing child folders.
- Confirm each manifest uses a supported `schema_version`.
- Read gallery and artwork manifests from the paths listed in the collection manifest.
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
- Preserve unknown external links and extension blocks when practical if the implementation later writes OAA archives.
- Validate all archive paths.
- Validate paths before Unicode normalization.
- Reject absolute paths.
- Reject parent-directory traversal.
- Reject manifests that contain apparent absolute local filesystem paths, including inside extension blocks.
- Ignore explicit ZIP directory entries when interpreting archive structure.
- Treat embedded artwork-associated files as untrusted. Avoid opening or rendering embedded files automatically.
- Treat media decoding and rendering as implementation-specific behavior outside OAA base conformance.

What the software does after reading the archive is outside the file-format specification.

## Writing Archives

Software that writes an `.oaa` archive generally needs to create a valid archive layout:

- Root `mimetype`
- Root `.oacollection`
- Gallery folders under `galleries/` containing `.oagallery`
- Artwork folders under `artworks/` containing `.oaartwork`
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
