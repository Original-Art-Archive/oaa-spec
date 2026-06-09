<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Format Overview

The Original Art Archive (OAA) Format is an archive/interchange format for original art collection folders.

An `.oaa` archive is a ZIP-compatible package of a portable collection folder. It contains a root `mimetype` identifier, a collection manifest, gallery manifests, artwork manifests, and artwork-associated files.

OAA is intended to allow independent collection platforms to exchange collection folders without binding the format to one application.

## Draft Layout

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

The collection manifest is the registry for gallery manifests and artwork manifests. Gallery manifests list artwork membership by archive-local artwork ID, so one artwork record can appear in multiple galleries without being physically duplicated.

Collection gallery order and gallery artwork order are represented by array order. The collection artwork list is a registry and may be used as fallback display order when no gallery context is available.

The `mimetype` file contains `application/vnd.original-art-archive+zip` and identifies the package as an Original Art Archive.

The 0.1 draft media type is provisional until registered with IANA.

For the 0.1 draft, all manifests use `schema_version` string `"0.1"`.

## Metadata Layers

OAA manifests use three metadata layers:

- Base fields for portable OAA meaning, such as collection names, gallery names, artwork titles, artist credits, private notes, and file references.
- `external_links` arrays for provider-local identifiers and URLs assigned by services such as ComicArtFans, SNIKT.com, and Raremarq.
- extension containers named `extensions` for provider-specific or application-specific fields that OAA does not standardize.

OAA base fields are designed to be portable. Semantic base fields use closed base value sets where readers need machine-interpretable meaning. Provider-specific values belong in extension blocks unless they can be mapped to valid OAA base values.

Display-oriented base strings, such as media, artwork type, creator role, sale-status text, and file format labels, are portable as text but are not controlled OAA machine values.

Readers must not reject external links only because their provider identifiers are unknown. Readers may display unknown external links generically and should preserve them when rewriting manifests, when practical.

Readers must ignore extension blocks they do not understand. Writers should preserve unknown extension blocks when rewriting manifests, when practical.

Embedded files are payloads, not media formats that OAA promises readers can decode. Readers should preserve embedded file metadata and bytes when practical, but decoding or rendering any file type is implementation-specific.
