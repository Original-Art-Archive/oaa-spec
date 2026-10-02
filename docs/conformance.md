<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Conformance

This page summarizes the four core classes in [SPEC.md](../SPEC.md).

| Class | Meaning |
| --- | --- |
| Valid Archive | Satisfies the 1.0 container, manifest, reference, path, field, and integrity requirements. |
| Structural Reader | Validates structure, resolves collection-authoritative records and embedded files, and implements required safety and processing behavior. |
| Metadata Reader | Also a Structural Reader; imports or displays base metadata and external links, including unknown providers. |
| Conforming Writer | Produces Valid Archives. |

Reader support includes Store, Deflate, and ZIP64 within declared capacities. Readers need not extract, decode, render, or preview media. Extracting readers need separate evidence for destination confinement, pre-existing filesystem redirections, and case/Unicode collision handling without silent overwrite.

## Results and Claims

An archive-validity violation makes an archive invalid. Advisory warnings alone do not. Optional recovery of usable records does not make the original archive valid, and skipped or lost content needs to be reported.

Unsupported versions, capacity limits, and destination limitations are separate processing outcomes. Stopping before validation completes cannot establish full validity.

The reference validator checks archive content and its own bounded-processing behavior. Passing it does not certify another implementation's safety, privacy, extraction, rendering, or preservation behavior. See the [release verification scope](release-1.0.0.md) for the exact tested scope.

## Preservation and Privacy

Unknown optional fields, external links, extension blocks, and embedded bytes should be preserved when practical. Import plus export is not evidence of lossless preservation. Claims need a stated scope, evidence, and limitations.

There is no best-effort “Round-Trip Implementation” conformance category in 1.0. The expanded preservation profile is deferred.

Private metadata, including its extensions, stays private unless separately authorized for disclosure. `is_public` is metadata, not authorization to publish every field or attachment. Unknown extensions and supporting files are not automatically public.

## Media Type

The identifier is `application/vnd.original-art-archive+zip`. This release makes no registration claim. Registration is administrative follow-up, not a release prerequisite.
