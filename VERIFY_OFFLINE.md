# Offline Verification

Pose Studio is designed so the generated standalone HTML can perform its application workflow without external runtime assets.

The first clean build requires network access to obtain the exact pinned npm package and the pinned Pose Landmarker model. Cached artifacts can be reused after that. The generated HTML embeds the required JavaScript, WASM, and model bytes.

## Main standalone HTML

1. Run `build-standalone.bat` on Windows.
2. Open `dist/index.html` directly and also test the GitHub Pages build over HTTPS.
3. Open browser developer tools and clear the Network panel.
4. Enable offline mode or disconnect the device after the HTML has loaded.
5. Reload the local `dist/index.html` where the target browser permits local camera access.
6. Confirm the UI, Japanese / English switching, help dialog, mobile bottom tabs, saved recording list, and sample/import flow load without an external resource request.
7. Start the camera, confirm the permission / live-preview flow, and confirm pose inference runs without HTTP / HTTPS requests. If the target browser does not permit camera use from `file://`, repeat the camera test from the HTTPS build while keeping DevTools offline after the initial HTML request.
8. Stop the camera and confirm the browser camera indicator disappears; restart the camera and repeat once.
9. Record a movement, finish it, replay the stick figure, seek, change playback speed, toggle loop / mirror, and use 2D / 3D when world landmarks are available.
10. Exercise Analyze: select different metrics / landmarks and confirm angle, range-of-motion, trajectory, speed, and left/right views update without console errors.
11. Exercise Compare with two recordings and confirm overall / form / timing / region results appear. Start a second comparison after changing one input and confirm no stale result from the first comparison replaces it.
12. Save a recording as both a pose reference and a motion reference. Start a reference challenge, create another recording, and confirm the comparison result opens for the intended reference.
13. Import valid `.pose.json` / `.pose-template.json` files and confirm malformed or oversized files fail safely.
14. Change the suggested output filename before every export and verify extension handling for `.pose.json`, `.pose-template.json`, `.csv`, and `.html`.
15. Export JSON, CSV, and standalone viewer HTML. Confirm each file contains the expected recording/reference and opens correctly.
16. Open the exported viewer HTML offline at desktop width and smartphone widths (including 320 px). Confirm there is no horizontal scrolling and that play/pause, seek, speed, loop, mirror, view, and joint controls work.
17. Confirm the main app Network panel contains no HTTP / HTTPS runtime request after the initial page load and that there are no unexpected console errors.
18. Clear browser site data only after the above tests; confirm the UI returns to the documented empty state.

The main application CSP uses `connect-src blob:` rather than `connect-src 'none'` because the embedded MediaPipe runtime reads in-memory Blob URLs for its embedded WASM/model initialization. It also includes `script-src 'wasm-unsafe-eval'` so the embedded WebAssembly module can be compiled. HTTP and HTTPS runtime connections remain blocked.

The exported read-only pose viewer does not need MediaPipe and uses `connect-src 'none'`.

## Self-extracting variant

1. Open `dist/index.self-extract.html` directly.
2. Confirm the loading-screen text and favicon are visible and the loader disappears.
3. Repeat the core offline workflow above, including camera behavior where the browser permits it.
4. Confirm the browser console contains no decompression or CSP errors.
5. Confirm the restored application behaves like `dist/index.html`.

`scripts/verify-self-extract.ps1` also checks the ASCII-only loader and byte-for-byte restoration of the readable HTML.

## Release evidence

Before release, retain or regenerate:

- `assets/screenshot.png` — Japanese desktop
- `assets/screenshot-en.png` — English desktop
- `assets/screenshot-mobile.png` — smartphone layout

Also inspect `dist/build-size-report.json` because the embedded pose runtime, WASM, and model make Pose Studio substantially larger than a dependency-free Browser Kitty app.
