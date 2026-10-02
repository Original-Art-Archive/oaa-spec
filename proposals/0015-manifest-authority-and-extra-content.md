<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Proposal: Manifest Authority, Layout, and Extra Content

## Status

Accepted for OAA 1.0

## Summary

Use manifests to define collection content, retain a recommended directory layout, and permit safe unreferenced extras and empty structures.

## Motivation

Directory names should not compete with manifests as a source of truth. Archives also need to represent small, empty, or metadata-only collections without inventing placeholder records or files.

## Specification Changes

- `galleries/` and `artworks/` remain the recommended layout, not the sole valid arrangement.
- The root collection manifest MUST determine the referenced gallery and artwork manifests. Readers MUST NOT infer additional collection records by scanning unreferenced content.
- Gallery membership MUST be explicit. Readers MUST NOT invent membership when it is absent.
- Artwork file paths MUST resolve within the directory containing that artwork's manifest; subdirectories MAY be used.
- Archives MAY contain safe unreferenced extra content. All entries, referenced or not, MUST satisfy archive safety requirements.
- The obligation to preserve all safe extras during unchanged import and export belongs to the [deferred preservation profile](0014-round-trip-preservation.md), not the 1.0 core contract. Archive safety and the requirements of each claimed core conformance class remain in force.
- Empty collections, empty galleries, metadata-only artworks, and artworks assigned to no gallery MUST be permitted when their required manifest structure is otherwise valid.

## Compatibility Impact

OAA 1.0 must distinguish recommended layout from required references and confinement. Schemas and validation must not impose nonempty membership or file lists contrary to this decision.

## Examples

An artwork can have an empty file list while preserving its descriptive metadata. A safe unreferenced text attachment does not become an artwork or gallery merely because it exists inside the archive.

## Open Questions

None for this decision. Alternate-layout, empty-structure, and extra-content fixtures remain to be integrated.
