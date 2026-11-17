# Static surface baseline — obs64.exe

**Target:** `C:\Program Files\obs-studio\bin\64bit\obs64.exe`
**Version:** 32.2.1 · **Publisher:** OBS
**SHA-256:** `b1541d18fd28a41e78251e885df1440eac619caf7bc00190c8cdcdd45c2bb7f5`
**Captured:** 2026-11-17 · **Tooling:** GNU objdump 2.47 via REA `inspect_with_objdump`

## Verified section map

Image base `0x140000000`. Eight sections, `pei-x86-64`.

| # | Name | Virtual size | VMA | File offset | Align | Flags |
|---|---|---|---|---|---|---|
| 0 | `.text` | `0x0034B7A1` (3,455,905) | `0x140001000` | `0x400` | 16 | CONTENTS, ALLOC, LOAD, READONLY, CODE |
| 1 | `.rdata` | `0x001BC9F4` (1,819,124) | `0x14034D000` | `0x34BC00` | 16 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 2 | `.data` | `0x00010800` (67,584) | `0x14050A000` | `0x508600` | 16 | CONTENTS, ALLOC, LOAD, DATA |
| 3 | `.pdata` | `0x0001839C` (99,228) | `0x14051E000` | `0x518E00` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 4 | `.detourc` | `0x000021C0` (8,640) | `0x140537000` | `0x531200` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 5 | `.detourd` | `0x00000018` (24) | `0x14053A000` | `0x533400` | 4 | CONTENTS, ALLOC, LOAD, DATA |
| 6 | `.rsrc` | `0x000175F0` (95,728) | `0x14053B000` | `0x533600` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 7 | `.reloc` | `0x00007AD8` (31,448) | `0x140553000` | `0x54AC00` | 4 | CONTENTS, LOAD, READONLY, DATA |

The post-read digest matched the pre-read digest (`artifact_changed: false`).

## Observations

- `.text` is ~3.3 MiB; the executable is a front-end host, not the whole engine
  (the core is the separate `obs.dll`).
- **`.detourc` (8,640 B, code) and `.detourd` (24 B, writable data)** are
  present. These are the payload sections emitted by Microsoft Detours.
- `.pdata` is ~97 KiB, consistent with C++ exception-heavy code.
- No `.tls` section appears in the table.

## Deductions

- The `.detour*` sections are strong evidence that Detours-style function
  hooking is compiled into this image, consistent with graphics/window capture
  hook injection used by capture plugins. The hook targets themselves are not
  identified by this baseline.
- The modest `.text` plus external `obs.dll`/Qt6/FFmpeg modules confirms the
  modular architecture already recorded in the README.

## Unknowns

- Which APIs are detoured, and whether the hooks are installed in `obs64.exe`
  or delegated to capture modules.
- Import table and module-loading path for the 31 plugin DLLs.
- Qt UI composition, encoder negotiation and the WebSocket/HTTP control surface
  remain unmapped.

## Provenance

Sections and sizes come directly from `objdump -h` output returned by REA
(evidence `ev_91946e57…bc6`). No code was executed; the file was read only.
