# FaultScope native CI validation record

Date: 2026-11-22  
Verification branch: `connector-access-verification-20261122`  
Repository commit tested: `18e4190eaecb58cbda8a1f3bcb026f148e1f1bfc`

The runs below are successful GitHub Actions executions on the verification branch. The branch was four commits ahead of `main` at the time of inspection; these results must not be represented as tests of the current `main` commit.

## x64dbg: PASS

Actions run: [#45013374671](https://github.com/Timesyou/FaultScope-Test-Lab-/actions/runs/45013374671)

| Architecture | Engine | Upstream regression tests | FaultScope bridge | Combined runner summary |
|---|---|---:|---:|---:|
| x86 | GleeBug | 82/82 | PASS (3 assertions) | 83/83 |
| x86 | TitanEngine | 82/82 | PASS (3 assertions) | 83/83 |
| x64 | GleeBug | 82/82 | PASS (3 assertions) | 83/83 |
| x64 | TitanEngine | 82/82 | PASS (3 assertions) | 83/83 |

Aggregate: **328 upstream regression-test passes** and **4/4 FaultScope bridge integration passes**. Each bridge integration test reported three passing assertions. Both architecture builds and all four test jobs completed successfully.

## Ghidra: PASS

Actions run: [#45013374574](https://github.com/Timesyou/FaultScope-Test-Lab-/actions/runs/45013374574)

- Pinned Ghidra source revision: `3fb3eec6a05442c901e9ead8620ca21eae830b18`.
- Source build: passed.
- Harmless C fixture compilation: passed.
- Real `analyzeHeadless` import and exporter execution: passed.
- `tests/ghidra/verify_export.py` report validation: passed.
- Report: 6,336 bytes; SHA-256 `988c1793771c360e717055686c24f9ce7b1f5d842d6ba727dd84584e826bc911`.
- Exported metadata: 19 functions, 7 imports, 84 strings, 59 references.

## Scope and limitations

These tests validate the supplied harmless fixture and the tested integration path. They do not establish that arbitrary binaries are safe, that all debugger event sequences are handled, or that every target environment behaves identically. The verification branch has not been merged into `main`; check the branch and commit above when reproducing these results.
