# FaultScope Native Test Lab

This repository runs native CI for the FaultScope Ghidra exporter and x64dbg plugin bridge.

## Repository contents

- `FaultScope-Test-Lab.zip` is the complete source/test payload: Java Ghidra exporter, C++ x64dbg plugin test, benign fixtures, validators and supporting notes.
- `.github/workflows/ghidra.yml` builds Ghidra from source and runs the exporter with `analyzeHeadless` against a compiled benign fixture.
- `.github/workflows/x64dbg.yml` builds x64dbg for x64 and x86, compiles the FaultScope bridge through x64dbg's own test target, then runs upstream regression suites and the FaultScope integration test on both architectures with TitanEngine and GleeBug.

The workflows unpack the payload into a temporary working directory. The archive is also downloadable from this repository if you want to inspect or extract it locally.

## Execution and pass criteria

A green build alone is not considered successful integration. The Ghidra job requires a real Ghidra-produced JSON report with expected metadata, functions, and strings. The x64dbg suite requires the plugin to load, capture a real process-created callback and PID, export a JSON session, and pass the report validator. All four native x64dbg architecture/engine combinations must complete successfully for the full matrix to pass.

The workflows run on push, pull request and manual dispatch. GitHub Actions must be enabled. No API tokens or other secrets are required. The x64dbg upstream source revision is resolved once and reused for build and test jobs; Ghidra's checked-out commit is shown in its Actions logs.

## Safety

Only the supplied harmless fixtures should be used in automated CI. Do not add private customer executables, incident dumps, internal paths, or sensitive investigation data to this public repository or its Actions artifacts. These tests validate the integration and exported observations; they do not determine whether arbitrary binaries are benign or malicious.

## Verified CI results (2026-11-22)

The first native x64dbg CI run passed on both architectures and both debugger engines:

- x86 + GleeBug: 82/82 upstream tests and the FaultScope bridge test passed.
- x86 + TitanEngine: 82/82 upstream tests and the FaultScope bridge test passed.
- x64 + GleeBug: 82/82 upstream tests and the FaultScope bridge test passed.
- x64 + TitanEngine: 82/82 upstream tests and the FaultScope bridge test passed.

The four downloaded logs and reports were independently inspected: 328 upstream regression test passes, 4/4 FaultScope bridge integration passes, valid JSON session reports, non-zero process IDs matching real process-created callbacks, module/thread/breakpoint/process-exit events, zero dropped events, validator success, and clean plugin unload. The independent verification uses line-based parsing so Windows CRLF logs are not undercounted.

The Ghidra source build and harmless fixture compilation passed on the first run. Its headless exporter step exposed an API compatibility error: `ReferenceManager.getReferencesFrom(Address)` returns `Reference[]`, not `ReferenceIterator`. The nested exporter has been corrected. The next native run must still pass headless execution and JSON validation before Ghidra can be marked successful.

Ghidra's source checkout is pinned to a specific commit for repeatable builds. The workflow uploads the JSON report rather than the 562 MB Ghidra distribution archive.
