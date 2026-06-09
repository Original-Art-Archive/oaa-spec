<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Contributing

OAA is currently a 0.1 Draft format. Changes should be proposed and reviewed against interoperability, reader safety, and backward compatibility.

## Contribution Licensing

By contributing to this repository, you agree that your contribution is licensed under the same license that applies to the file or directory you are modifying.

For example:

- Contributions to `SPEC.md`, `README.md`, or `docs/` are licensed under CC BY 4.0.
- Contributions to `examples/` are dedicated under CC0 1.0.
- Contributions to machine-readable schema files are licensed under CC0 1.0, or alternatively MIT when incorporated into software.
- Contributions to scripts or other software, if added, are licensed under MIT.

Do not contribute material unless you have the right to license it under the applicable license.

## Format Changes

Format changes should be proposed using [proposals/0000-template.md](proposals/0000-template.md).

Changes that affect valid archives or reader behavior should update these files together:

- [SPEC.md](SPEC.md)
- [docs/](docs/)
- [schema/](schema/)
- [examples/](examples/)
- [requirements/](requirements/)

Validator behavior changes should be coordinated with the [OAA validator repository](https://github.com/Original-Art-Archive/oaa-validator).

## Review Criteria

Reviewers should evaluate:

- Whether the change preserves safe archive reading and extraction.
- Whether the change is portable across Windows, macOS, Linux, and web services.
- Whether unknown fields and extension block data can still be preserved by round-trip implementations.
- Whether examples and requirements traceability remain aligned with the spec.
- Whether private collector metadata remains private by default.

## Draft Compatibility

Versions before 1.0 may change incompatibly, but draft changes should still document their compatibility impact.
