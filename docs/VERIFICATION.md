# Verification — Motion Notes 2.0.0

Date: 2026-10-08 JST. Source: supplied v1.1.0 ZIP. No public deployment or remote CI run was performed.

## Executed

- Current template `scripts/check-repository.ps1` under PowerShell 7.4.6 on Linux (tar exposed as tar.exe): PASS. Includes syntax/encoding preflight, empty/single-item dependency checks, generated standalone verification, self-extract byte recovery and root HTML byte identity.
- `node --test tests/data-contract.test.cjs`: 11 passing tests covering invalid dates/reference text, first missing detection and the five-minute boundary, plus legacy data, malformed coordinates/timestamps, missing-pose gaps, timed-note validation, reference formats and bounds.
- Real headless Chromium 153 at 1360×900 and 360×800, plus 360×420 help dialog: three workflow pages; no horizontal overflow; dialog scroll/Escape; Japanese/English.
- Notes create, edit, jump, delete/undo, reload persistence; edited filename survives language change; malformed import does not replace current data; final-recording deletion disables exports.
- Actual JSON/CSV/HTML/PNG downloads: editable Unicode filename, JSON notes/landmarks, PNG signature, viewer note jumping and safe text. The viewer's hostile-looking note text remains text, never a script element.
- Legacy-reference save and two-recording comparison; joint-angle display.
- Readable and self-extracting HTML opened directly with file://. UI/export tests observed no external requests or uncaught page errors.
- Actual MediaPipe inference on a 2-second local test video: 24 samples on first and second imports after sequential-sampling change. Original runtime/model pinned and embedded. No external logging request after sender-disable transform.
- HTTP camera/worker path using Chromium's fake-camera source (a real person test image converted locally to video): pose detection, countdown, 29 samples recorded in the observed run; navigation to review released camera; subsequent local video import produced 24 samples. A separate permission-denied → retry → capture → restart → video run also passed (20 capture samples, 24 video samples). No external request or uncaught page error.

## Reproduce

On Windows: `build-standalone.bat`, then `powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts/check-repository.ps1`.

Core tests: `node --test tests/data-contract.test.cjs`.
Browser tests require Playwright in CODEX_PRIMARY_RUNTIME_NODE_MODULES (or adapt require to your installed Playwright), CHROMIUM_PATH to a local executable, and optionally TEST_OUTPUT. `node tests/browser.cjs` writes actual representative screenshots and uses temporary synthetic recordings. No test hooks or injected app state are needed.

## Not verified / limitations

- No physical webcam or real smartphone, iOS/Safari, Firefox or Windows PowerShell 5.1 device run. Mobile widths were emulated in Chromium. Real-device release checks remain advisable.
- Synthetic sample is illustrative, not measured movement. Inference fixture is a static human image made into a short video, not a motion-accuracy benchmark.
- Similarity, world coordinates and joint angles remain estimates. Tests establish operation and data handling, not biomechanical accuracy.
- Five-minute inference performance and device thermal/battery behavior were not measured. Source length/data bounds and cancellation logic are covered separately from hardware throughput.
- Single-file hosting or changing local-file paths may change browser storage origin behavior; use JSON backups when moving installations.
