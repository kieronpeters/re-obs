# re-obs

An independent, clean-room reconstruction of a scoped subset of OBS Studio's
recording architecture: a capture → composite → encode → mux pipeline with a
minimal plugin interface.

> **Not affiliated with, endorsed by, or derived from the source code of the OBS
> Project.** No third-party binaries, disassembly listings, extracted resources,
> or copied source are included in this repository.

## Target under analysis

| Field | Value |
|---|---|
| Application | OBS Studio |
| Version | 32.2.1 |
| Publisher | OBS |
| Executable | `C:\Program Files\obs-studio\bin\64bit\obs64.exe` |
| Size | 5,589,264 bytes |
| Format | PE32+ (`pei-x86-64`), x86-64 |
| SHA-256 | `b1541d18fd28a41e78251e885df1440eac619caf7bc00190c8cdcdd45c2bb7f5` |
| Baseline captured | 2026-11-16 |

Identity, version, size and PE architecture were verified from registry data,
file metadata and a static `objdump -f` header read. The digest recorded before
and after the read was unchanged.

## Verified component baseline

| Component | Evidence |
|---|---|
| `obs.dll` (1,176,848 B) | core engine module, version 32.2.1 |
| `Qt6Core.dll` (10,412,304 B, Qt 6.11.1) | Qt 6 UI/runtime |
| `obs-plugins\64bit\*.dll` | **31 plugin DLLs** present |
| `avcodec-62`, `avformat-62`, `avutil-60`, `avfilter-11`, `avdevice-62` | FFmpeg 6x/7x-generation media libraries |
| `libobs-d3d11.dll` | Direct3D 11 graphics backend |
| `libcurl.dll`, `datachannel.dll` | networking / WebRTC data channel libraries |

Role assignments follow from module names; the internal ABI and threading model
remain **unknown** until analysis.

## Objective

Reconstruct, with documented fidelity limits:

1. A source abstraction and a scene/composite graph.
2. A desktop/window capture source (Windows Graphics Capture or DXGI duplication).
3. A video encoder stage and an audio path.
4. A container muxer producing a playable file.
5. A minimal plugin/module interface and configuration model.

## Non-goals

- Full streaming stack (RTMP/SRT/WebRTC), scene transitions, filters, hotkeys.
- Byte-identical output or encoder parity.
- Redistributing OBS binaries, plugins, FFmpeg binaries, or extracted assets.

## Method and evidence policy

Static-first: PE metadata, module inventory, Ghidra 12.1.4 headless import of
`obs64.exe` and `obs.dll`, string/import/cross-reference analysis, and
cross-module correlation. The target is **not executed** during analysis;
dynamic instrumentation (Frida) only if separately authorised.

Evidence tags used in `docs/`: **Verified**, **Deduction**, **Hypothesis**,
**Unknown**.

## Reconstruction scope and risk

The full product is far beyond a single project. The realistic deliverable is a
**minimal recorder**: one capture source, one encoder, one muxer, one output
file, driven by a configuration file or small UI. Graphics and encoder behaviour
may not be fully observable from static analysis, so some stages will be built
against public API documentation and black-box observation.

## Repository layout

```
README.md          this file
LICENSE            Apache-2.0
ROADMAP.md         phased plan and exit criteria
docs/              evidence, architecture notes, unknowns
src/               implementation (added in Phase 2)
tests/             automated tests (added in Phase 2)
tools/             analysis helper scripts (no binaries)
```

## Status

Phase 0 — scope, methodology and verified evidence baseline. No code yet.

## Licence

Apache-2.0. See `LICENSE`. The licence covers original work in this repository
only; it grants no rights to the original OBS Studio software.
