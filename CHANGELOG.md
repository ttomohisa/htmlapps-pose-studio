# Changelog

## 2.0.0 — 2026-10-08

- Reframed Pose Studio as **Motion Notes / 動きのノート**: capture, review and carry a movement notebook.
- Three true workflow pages on desktop/mobile; existing joint analysis, comparison and references remain available as optional tools.
- Timestamped notes with editing, seeking, deletion and undo; persisted and included in JSON and portable HTML.
- PNG pose sheets from marked moments, or six automatic moments when no notes exist.
- Sequential 12 Hz local-video sampling; preserves source aspect ratio and missing-pose intervals.
- Strict JSON validation, transaction-complete persistence, retained user export filenames, disabled empty-state exports.
- Current template snapshot cb908779682fa315ccd0f1eb58549f6c208f36f0: preflight, root HTML, canonical icon, components and preview workflows, with app-specific model/SRI support retained.
- Disabled the pinned runtime telemetry sender with a fail-closed build transform; restrictive CSP remains.
- Updated bilingual help/readmes and screenshots. Legacy schema, database and repository identity retained.



## [1.1.0] - Unreleased

### Added

- Import local video files and convert them into pose recordings without saving the source video.
- Show video-analysis progress and allow canceling long processing.

### Fixed

- Auto-fit 3D playback to the recorded world-landmark bounds so the stick figure stays inside the viewport.
- Apply the same 3D auto-fit behavior to exported standalone HTML viewers.

## [1.0.3] - Unreleased

### Fixed

- Clear the live pose overlay immediately when the camera is stopped.
- Restore the template-standard borderless header icon buttons.

### Changed

- Add a first-use guide that explains the three-step recording flow and what can be done after recording.
- Add a dynamic “what to do next” guide during camera setup, framing, recording, and completion.
- Hide runtime statistics under a details section so the primary workflow stays focused.
- Require a detectable body before enabling recording.
- Add direct navigation back to Record from empty Analyze / Compare / Reference states.
- Add a subtle camera-free sample path for first-time exploration.

## [1.0.2] - Unreleased

- Fix direct-file (`file://`) camera usage by avoiding Blob module Worker startup on opaque origins.
- Use MediaPipe's classic `vision_wasm_internal` runtime for the embedded standalone build.
- Add an automatic main-thread pose inference fallback when Worker startup fails.
- Throttle the main-thread fallback to about 15 fps so controls and drawing remain usable.
- Keep Worker-based inference when the app is served from HTTP(S).

All notable changes to Pose Studio are documented here.

## [1.0.1] - Unreleased

### Fixed

- Allow MediaPipe WebAssembly compilation under CSP with `wasm-unsafe-eval`.
- Wait for the pose worker `ready` signal before treating runtime initialization as complete.
- Surface pose-runtime initialization failures instead of leaving the camera status stuck on “preparing”.
- Restore the pinned MediaPipe runtime and Pose Landmarker model to the latest template dependency pipeline.

## [1.0.0] - Unreleased

### Added

- Camera-based 33-point pose recording without source-video recording.
- Stick-figure playback in 2D and 3D.
- Joint-angle, trajectory, speed, and left/right analysis.
- Two-recording comparison with normalization and DTW alignment.
- Pose / motion reference templates and challenge scoring.
- IndexedDB persistence and JSON / CSV import-export.
- Standalone HTML viewer export containing viewer code and pose data together.
- Japanese / English UI and smartphone bottom-tab navigation.
- Fully embedded MediaPipe runtime, WASM, and pinned Pose Landmarker Lite model for runtime-offline use.
