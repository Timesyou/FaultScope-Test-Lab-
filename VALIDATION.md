# FaultScope native CI validation record

Date: 2026-11-22
Repository commit tested: `fd263d4ed1e31da543bfa56a5b0300f0e280e664`

## x64dbg: independently verified PASS

| Architecture | Engine | Upstream regression tests | FaultScope bridge | Events | Dropped |
|---|---|---:|---:|---:|---:|
| x86 | GleeBug | 82/82 | PASS (3 assertions) | 16 | 0 |
| x86 | TitanEngine | 82/82 | PASS (3 assertions) | 16 | 0 |
| x64 | GleeBug | 82/82 | PASS (3 assertions) | 16 | 0 |
| x64 | TitanEngine | 82/82 | PASS (3 assertions) | 15 | 0 |

Totals: **328 upstream regression test passes** and **4/4 FaultScope bridge integration passes** (332 test-case passes including the four bridge tests).

Each downloaded GitHub Actions artifact ZIP passed ZIP integrity checking. The 82 upstream debug logs in each combination each contained exactly one passing final test line. Each plugin log and session report was independently checked for plugin load, both registered commands, process-created callback, matching non-zero process ID, module/thread events, breakpoint event, process exit, final debug-stop, JSON schema, zero dropped events, repository validator success, and clean plugin unload. Each bridge run satisfied three integration assertions.

These results validate the supplied benign debuggee fixture and this tested integration path. They do not prove that every possible executable, debugger event sequence, or target environment will behave identically.

## Ghidra: compatibility fix included; rerun required

The first CI run successfully checked out Ghidra, configured its required JDK, fetched dependencies, built Ghidra from source, and compiled the benign fixture. The real headless exporter step then failed at script compilation because `ReferenceManager.getReferencesFrom(Address)` returns `Reference[]`, not `ReferenceIterator`.

The nested exporter in this repair now iterates the returned `Reference[]`. The workflow is pinned to Ghidra commit `3fb3eec6a05442c901e9ead8620ca21eae830b18`, and the large Ghidra distribution archive is no longer uploaded as an artifact. Ghidra is **not considered passed** until the new Actions run completes headless analysis, creates the report, and passes `tests/ghidra/verify_export.py`.
