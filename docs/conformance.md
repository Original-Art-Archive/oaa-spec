<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Conformance

This page summarizes draft OAA conformance levels for archives and parsers. The source of truth is [../SPEC.md](../SPEC.md).

## Valid Archive

A Valid Archive follows the container, `mimetype`, layout, manifest, path, and field rules in the specification.

A Valid Archive uses manifest `schema_version` string `"0.1"` for this draft.

A Valid Archive uses the draft media type `application/vnd.original-art-archive+zip`. The media type is provisional until registered with IANA.

A Valid Archive does not contain encrypted archive entries.

A Valid Archive stores each `files[]` entry as an embedded archive file and keeps collection gallery/artwork reference IDs and paths unique.

Within each gallery manifest, artwork membership IDs are unique.

## Reader Conformance Levels

Reader support is described with narrower levels rather than one broad reader claim.

Reader conformance levels are cumulative unless otherwise stated.

### Structural Reader

A Structural Reader can validate package structure, parse manifests, resolve galleries and artworks, and locate embedded files according to the specification.

Structural Readers do not need to decode, render, preview, or open any embedded file type.

Structural Readers support Store and Deflate ZIP compression methods.

### Metadata Reader

A Metadata Reader can import or display OAA base metadata and external links.

Metadata Readers must not reject unknown external link providers. They should preserve unknown external links and extension blocks when practical.

A Metadata Reader is also a Structural Reader.

### Round-Trip Implementation

A Round-Trip Implementation can read OAA and later write OAA while preserving unknown optional fields, unknown external links, and unknown extension blocks when practical.

A Round-Trip Implementation is both a reader and writer.

## Embedded File Handling

OAA conformance does not require support for decoding JPEG, PNG, TIFF, PDF, video, or any other embedded data file type.

Readers validate archive structure, manifests, references, and path safety. Opening or rendering embedded files is implementation-specific and at the reader's or user's risk.

Readers should preserve embedded file metadata and bytes when practical, even when the reader cannot decode or render the file.

## Conforming Writer

A Conforming Writer produces Valid Archives, including the required root `mimetype`, according to the specification.
