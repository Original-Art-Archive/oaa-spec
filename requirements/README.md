<!-- SPDX-License-Identifier: CC-BY-4.0 -->

# Requirements Traceability

The normative rules are in [SPEC.md](../SPEC.md). The supporting [oaa-1.0.yaml](oaa-1.0.yaml) catalog assigns stable requirement IDs; [traceability.md](traceability.md) maps them to rules and fixtures. The [0.1 catalog](oaa-0.1.yaml) remains unchanged for historical use.

Automated rules cover archive/content conditions and explicitly classified processing outcomes. Processing findings (unsupported version, configured capacity, unavailable input) are not archive-invalidity findings. Reader, writer, privacy, and other behavior that cannot be inferred from an archive is tracked separately, with evidence in the [release verification scope](../docs/release-1.0.0.md).

The tests require every automated requirement to have rule and expected-finding fixture coverage, and every finding to reference known requirement IDs. Fixture coverage does not prove another application's implementation behavior.

Regenerate and check the matrix from a checkout of the [validator repository](https://github.com/Original-Art-Archive/oaa-validator):

```powershell
python requirements/generate_traceability.py
python requirements/generate_traceability.py --check
```
