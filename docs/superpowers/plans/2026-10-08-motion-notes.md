# Motion Notes Implementation Plan

> For agentic workers: use superpowers:executing-plans inline, with final independent review.

**Goal:** turn Pose Studio into a focused movement notebook, preserving its core engines.
**Architecture:** retain existing embedded MediaPipe/data/analysis implementations; reorganize DOM into task pages and add optional note data and shareable contact sheet. Adopt current template tooling with documented app extensions.
**Tech Stack:** single HTML, Canvas, IndexedDB, MediaPipe, PowerShell, Node browser tests.
**Spec:** ../specs/2026-10-08-motion-notes.md

## Global constraints
No upload/runtime CDN; #16624F; JA/EN; preserve schemaVersion 1 and original IndexedDB; source ZIP authority; no merge/publish.

## Review focus
- Last recording deletion must invalidate all export buttons.
- Gaps must not invent poses.
- Malformed JSON may not poison storage.
- Filename edits must survive translation/render calls.
- Slow/cancelled media processing must not leave camera or stale work active.

## Task 1: template and build
- [x] Compare current template and preserve remote-model + integrity extensions in builder/checks.
- [x] Adopt latest generic components/workflows/preflight and icon placeholders.
- [x] Build both outputs and verify hashes and network policy.

## Task 2: movement notebook
- [x] Write behavioral tests for notes/roundtrip, invalid import, missing pose, filenames and last deletion; watch failures.
- [x] Restructure src/index.template.html into capture/review/library and secondary preserved tools.
- [x] Add optional timestamped notes, note jumping/removal, PNG sheet and HTML notes.
- [x] Implement strict data validation and gap preservation; confirm tests pass.

## Task 3: verification and delivery
- [x] Test main flows in real Chromium, file + HTTP, both variants, JA/EN at desktop/mobile.
- [x] Test model init/video inference and cancellation; document physical-camera limitation.
- [x] Update APP_SPEC/readmes/help/changelog/security; capture actual UI screenshots.
- [x] Run repository check and final review; fix concrete findings.
- [x] Prepare validated release ZIP. Final artifact writeback and link are recorded in the delivery session.
