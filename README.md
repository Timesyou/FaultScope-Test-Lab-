# FaultScope Native Test Lab

This repository runs native CI for the FaultScope Ghidra exporter and x64dbg plugin bridge.

## Repository contents

- `FaultScope-Test-Lab.zip` contains the Java Ghidra exporter, C++ x64dbg plugin test, harmless fixtures, validators, and supporting notes.
- `.github/workflows/ghidra.yml` builds Ghidra from source and runs the exporter with `analyzeHeadless` against a compiled benign fixture.
- `.github/workflows/x64dbg.yml` builds x64dbg for x64 and x86, compiles the FaultScope bridge through x64dbg's own test target, then runs upstream regression suites and the FaultScope integration test on both architectures with TitanEngine and GleeBug.

The workflows unpack the payload into a temporary working directory. The archive is also downloadable from this repository if you want to inspect or extract it locally.

## Execution and pass criteria

A green build alone is not considered successful integration.

- The Ghidra job requires real headless analysis, a Ghidra-produced JSON report, and successful report validation.
- The x64dbg job requires successful x86 and x64 builds, then successful upstream and FaultScope bridge tests for both TitanEngine and GleeBug.
- The x64dbg workflow resolves the upstream `development` revision once and reuses that exact SHA for build and test jobs.
- GitHub Actions must be enabled. The workflows do not require repository secrets.

## Safety

Only the supplied harmless fixtures should be used in automated CI. Do not add private customer executables, incident dumps, internal paths, or sensitive investigation data to this public repository or its Actions artifacts. These tests validate the integration and exported observations; they do not determine whether arbitrary binaries are benign or malicious.

## Verified CI results — 2026-11-22

The successful verification runs below were executed on branch `connector-access-verification-20261122`, commit `18e4190eaecb58cbda8a1f3bcb026f148e1f1bfc`. They are real GitHub Actions runs, not local simulations. The verification branch is ahead of `main` by four commits; these results do not imply that those commits have been merged into `main`.

### x64dbg

[Successful Actions run #45013374671](https://github.com/Timesyou/FaultScope-Test-Lab-/actions/runs/45013374671)

- Both x86 and x64 builds completed successfully.
- All four architecture/engine test combinations completed successfully.
- Each combination reported **83/83 tests passed**: 82 upstream tests plus the FaultScope bridge integration test.
- The bridge integration test passed with 3 assertions in each of the four combinations.
- Aggregate: 328 upstream regression-test passes and 4/4 FaultScope bridge integration passes.

### Ghidra

[Successful Actions run #45013374574](https://github.com/Timesyou/FaultScope-Test-Lab-/actions/runs/45013374574)

- Ghidra was checked out at pinned commit `3fb3eec6a05442c901e9ead8620ca21eae830b18`.
- Source build, benign fixture compilation, headless analysis, exporter execution, and JSON validation all passed.
- The exported report recorded 19 functions, 7 imports, 84 strings, and 59 references.
- Report SHA-256: `988c1793771c360e717055686c24f9ce7b1f5d842d6ba727dd84584e826bc911`.

These results apply to the tested source revision, workflow configuration, and benign fixture. They do not prove that every executable, debugger event sequence, or target environment will behave identically.
