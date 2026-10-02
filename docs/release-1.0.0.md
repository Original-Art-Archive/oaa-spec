<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# OAA 1.0.0

Specification release **1.0.0** uses manifest version **"1.0"**. See the
[specification](../SPEC.md), [migration guide](migration-0.1-to-1.0.md), and
[decision index](../proposals/README.md). The base field set is frozen; deferred
features are not part of this release.

The [reference validator](https://github.com/Original-Art-Archive/oaa-validator)
implements 1.0 only. Unsupported versions and incomplete processing are distinct
from invalid archives. Historical 0.1 schemas, catalogs, and public tags are retained.

## Verification scope

The reference suite checks manifests, paths, identities, unknown optional/provider
data, primitive values, embedded sizes, Store/Deflate and ZIP64 handling, bounded
processing, version dispatch, and malformed input. It includes the 65,537-byte
Deflate trailing-garbage regression and legacy BZIP2 version-dispatch regression.
Distribution checks exercise installed-package imports and its bundled schema.

The requirement catalog contains 117 requirements, including 89 with automated
rule and expected-finding coverage. The fixtures include five valid definitions
and 77 mutation definitions. Tests check source-byte preservation, prohibit
extraction/network/process-launch calls in the privacy check, and verify that
private marker values do not appear in diagnostics. Diagnostics remain potentially
private local output.

The reference implementation does not extract, render, publish, or migrate
collections. Verification does not certify any application's extraction safety,
privacy controls, lossless preservation, or independent interoperability. Testing
recorded for this release preparation used Windows and Python 3.12; it is not a
claim of completed macOS/Linux verification. No application must adopt OAA as a
condition of this release.

## Downloads and checksums

Release assets are listed on the
[specification release](https://github.com/Original-Art-Archive/oaa-spec/releases/tag/v1.0.0)
and [validator release](https://github.com/Original-Art-Archive/oaa-validator/releases/tag/v1.0.0).
Use each release's `SHA256SUMS` to check its attached downloads. GitHub-generated
source downloads are separate from the explicitly named, checksummed source bundles.

The four example `.oaa` files contain synthetic collection metadata and approved
fixture media. Their embedded `SOURCE.md` files retain source attribution and
rights information; third-party media is not relicensed. The repository's
[split licenses and names-and-marks policy](../LICENSE.md) remain in force.

## Follow-ups

Independent exchange, the expanded preservation profile, deferred metadata, and
MIME registration remain separate follow-ups. This release makes no IANA
registration claim. See [open questions](../OPEN_QUESTIONS.md).
