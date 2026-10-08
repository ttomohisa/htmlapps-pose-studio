# Motion Notes / 動きのノート — 2.0.0

## Purpose
Turn a camera session or local video into a portable movement notebook: replay a skeleton, annotate important moments and export it without source footage. Formerly Pose Studio. This is an observation aid, not a form-rating coach or medical measurement tool.

## Workflow
Three true pages at every width: Capture, Review, Notebooks. Capture offers video, camera and a synthetic sample. Review contains a player with nearby transport/seek controls, notes and exports. Notebooks lists the browser's recordings. Joint analysis, two-recording comparison and saved pose/motion references remain optional review tools, on separate pages with a back action.

## Preserved core
MediaPipe Pose Landmarker Lite; one person; 33 normalized landmarks and world landmarks where available. Camera counts down, records timestamped landmarks up to five minutes, and never records pixels/audio. HTTP camera inference uses a worker with main-thread fallback; direct-file mode uses the main thread. Video import processes up to five minutes at requested 12 Hz using sequential seeks so inference does not race realtime playback. Unsupported codecs produce an error, cancellation releases the temporary source URL. Imported video is not persisted. Source aspect ratio is saved for correct 2D rendering; legacy files use 4:3 when unknown.

Playback has play/pause, seek, 0.5x/1x/2x, loop, mirror and world-coordinate 3D rotation when available. Missing samples and intervals over 350 ms are blank, never extrapolated into a fabricated pose. Analysis preserves joint-angle summaries, trajectory and estimated speed. Comparison retains normalized DTW alignment and separately labeled form/timing similarity. References can hold a pose or a movement, body regions, mirror matching and notes.

## Notebook data
Keep IndexedDB `browser-kitty-pose-studio`, version 1, stores recordings and references. Keep `.pose.json` / `.pose-template.json`, `format: browser-kitty-pose`, schemaVersion 1, kind recording/reference. New optional recording field: `notes: [{id, t, text}]`, timestamp in milliseconds, at most 24 notes, at most 240 characters each. Add, edit, jump, delete and undo deletion. Edits preserve timestamp. All note writes wait for an IndexedDB transaction commit. Existing recordings without notes remain valid. Single-pose references do not inherit out-of-range timed notes.

## Import boundaries
JSON limit 25 MiB; finite duration <=300050 ms; 1–20000 ordered samples; at least one valid pose. Landmarks have exactly 33 points with finite x/y/z, and bounded confidence when present. Explicit missing samples are accepted. Invalid metadata, time ranges, duplicate note IDs and unsupported schema/kinds are rejected before storage. Imported records receive new local IDs. Original data is unchanged on import failure.

## Exports
Editable base filename, sanitized separators/control characters and predictable extensions. Preserve user filename edits across language changes and rerendering.
- JSON: reusable complete landmarks and timed notes.
- CSV: timestamped landmark/world-coordinate columns, one row per sample.
- HTML: standalone read-only player, notes jump links, metadata, joint-angle readout; no model/camera/source video. Notes rendered using DOM textContent, and name escaped. No external dependencies.
- PNG: three-column pose contact sheet of all notes (up to 24), timestamps and text. Six evenly spaced moments when no notes exist. Uses current mirror/world-view settings. Filename editable like other exports.
Exports and note creation are disabled when no recording is selected.

## Privacy and build
No backend, account, runtime CDN, analytics or source uploads. Pose data is not anonymous. Keep third-party notices. Disable the pinned runtime's telemetry sender at build time with an exact-match guard; keep CSP blocking external connections. Embed model, WASM, runtime, styles, SVG and code. Produce dist/index.html, dist/index.self-extract.html and root pose-studio.html. Only source/config/build scripts may be edited; output is generated. Upstream snapshot and extensions are in docs/TEMPLATE_MIGRATION.md.

## UI and accessibility
Japanese/English, light theme, #16624F. SVG icons, visible focus, labeled controls, 360px support, safe-area mobile navigation. Preview controls remain next to previews. Dialog bodies scroll within viewport; Escape/backdrop/close button work. New inference is user-initiated. Browser storage clearing can lose notes; recommend JSON backups.

## Validation
See docs/VERIFICATION.md for performed and unverified tests. Regression source in tests/data-contract.test.cjs and tests/browser.cjs. Screenshots show the rendered app, not mockups. Physical camera and Safari/iOS need device-level release checks; do not imply they were tested by desktop emulation.
