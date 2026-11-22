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

## Current state

The harness package and workflow definitions have been assembled and statically checked. Native builds, plugin loading, and actual report export are **pending the first GitHub Actions run**. Read the individual job logs and download the artifacts; do not treat a missing artifact or skipped test as a pass.
