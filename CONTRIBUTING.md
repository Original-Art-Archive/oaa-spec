<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Contributing

OAA 1.0.0 uses manifest version `"1.0"`. Changes should be reviewed against interoperability, reader safety, privacy, and backward compatibility.

## Contribution Licensing

By contributing to this repository, you agree that your contribution is licensed under the same license that applies to the file or directory you are modifying.

For example:

- Contributions to `SPEC.md`, `README.md`, or `docs/` are licensed under CC BY 4.0.
- Contributions to `examples/` are dedicated under CC0 1.0.
- Contributions to machine-readable schema files are licensed under CC0 1.0, or alternatively MIT when incorporated into software.

Do not contribute material unless you have the right to license it under the applicable license.

## Format Changes

Format changes should be proposed using [proposals/0000-template.md](proposals/0000-template.md).

Changes that affect valid archives or reader behavior should update these files together:

- [SPEC.md](SPEC.md)
- [docs/](docs/)
- [schema/](schema/)
- [examples/](examples/)
- [requirements/](requirements/)

Coordinate implementation and test changes with the separate [validator repository](https://github.com/Original-Art-Archive/oaa-validator), whose contributions follow its own licensing policy.

## Review Criteria

Reviewers should evaluate:

- Whether the change preserves safe archive reading and extraction.
- Whether the change is portable across Windows, macOS, Linux, and web services.
- Whether implementations can preserve unknown data, and whether any preservation claim is backed by scoped evidence.
- Whether examples and validator expectations remain aligned with the spec.
- Whether private collector metadata remains private by default.

## Draft Compatibility

Versions before 1.0 may change incompatibly, but draft changes should still document their compatibility impact.

The base field set is frozen for 1.0. Deferred fields and the expanded preservation profile are not release prerequisites. New proposals should explain a concrete shared need, examples, and compatibility impact; no minimum implementation count is required. See [versioning](docs/versioning.md) for the 1.x compatibility and patch guarantees.
