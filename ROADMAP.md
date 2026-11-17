# Roadmap - re-obs

## Phase 0 - Scope and baseline (complete)
Deliverables: README, licence, verified evidence baseline, ignore rules.
Exit criteria: target identity and digest recorded; scope and non-goals agreed.

## Phase 1 - Module and ABI map
Ghidra import of `obs64.exe` and `obs.dll`; inventory the 31 plugin DLLs;
recover module registration and configuration flow.
Deliverable: `docs/architecture.md`.
Exit criteria: a documented hypothesis for the plugin/module interface.

## Phase 2 - Capture prototype
Standalone Windows Graphics Capture or DXGI duplication producing frames.
Exit criteria: stable 30 fps capture of one window for 60 seconds.

## Phase 3 - Encode and mux prototype
One encoder and one container using public APIs or bundled-library equivalents;
record a playable file independent of OBS.
Exit criteria: output validates in an independent player/prober.

## Phase 4 - Minimal composite and plugin interface
Source abstraction, scene graph, module loading with a documented ABI.

## Phase 5 - Configuration and control
Config file plus CLI or minimal UI; start/stop recording.

## Phase 6 - Hardening and documentation
Long-run stability, dropped-frame accounting, final write-up with open questions.
