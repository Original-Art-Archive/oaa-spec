<!-- SPDX-License-Identifier: CC-BY-4.0 -->

<p align="center">
  <img src="assets/oaa-logo.svg" alt="Original Art Archive logo" width="180">
</p>

# Original Art Archive Format

> Status: 0.1 Draft. This format is published for review and early implementation feedback. It is not the final 1.0 specification.

The Original Art Archive (OAA) Format is a draft archive and interchange format for transferring original art collection folders between collection platforms.

An `.oaa` file is a ZIP-based archive container with a defined internal structure and a root `mimetype` file identifying it as an Original Art Archive package.

In practical terms, an `.oaa` archive is a portable collection folder packaged as one file. The archive contains a `mimetype` identifier, a collection manifest, gallery manifests, artwork manifests, and the files associated with each artwork. Artwork records live in a collection-level artwork registry so the same artwork can appear in more than one gallery.

OAA metadata separates portable base fields from external links and extension blocks. This lets tools share common data while preserving provider-specific fields for services such as ComicArtFans, SNIKT.com, and Raremarq.

This repository is maintained by Remgrandt Games LLC.

## Repository Contents

- [SPEC.md](SPEC.md) defines the archive format and requirements outside manifest-local JSON validation.
- [docs/](docs/index.md) contains supporting documentation for implementers.
- [examples/](examples/README.md) contains draft example folder layouts and manifests.
- [schema/](schema/README.md) contains the normative JSON Schema for OAA 0.1 dot-manifest structure.
- [requirements/](requirements/README.md) contains validator requirement traceability for the 0.1 Draft.
- [proposals/](proposals/README.md) contains the proposal process for draft format changes.
- [assets/](assets/README.md) contains OAA visual assets and mark-use guidance.
- [LICENSES/](LICENSES/) contains the full license texts.
- [CHANGELOG.md](CHANGELOG.md), [CONTRIBUTING.md](CONTRIBUTING.md), [SECURITY.md](SECURITY.md), and [OPEN_QUESTIONS.md](OPEN_QUESTIONS.md) describe release history, contribution expectations, security policy, and draft questions.

Implementers should treat [SPEC.md](SPEC.md) as authoritative for the archive format as a whole. The JSON Schema is authoritative for manifest-local JSON structure.

The reference validator is staged separately at [Original-Art-Archive/oaa-validator](https://github.com/Original-Art-Archive/oaa-validator).

OAA is intended to be a neutral interchange format. It is not owned by any one application or platform.

## License

The OAA repository uses a split-license model:

- Specification and documentation: CC BY 4.0
- Examples, fixtures, and sample archives created for this repository: CC0 1.0
- Machine-readable schemas: CC0 1.0, or MIT when incorporated into software

Third-party or public-source media included or referenced by examples is governed by its original source rights statement and is not relicensed by Remgrandt Games LLC unless explicitly stated in the example directory.

The names "Original Art Archive" and "OAA" may be used to truthfully describe compatibility with the format, but may not be used to imply official status, endorsement, certification, or maintenance by the OAA project or Remgrandt Games LLC.

See [LICENSE.md](LICENSE.md) for details.
