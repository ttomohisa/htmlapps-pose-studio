# Security Policy

## Supported version

Security fixes target the latest version on the default branch.

## Reporting a vulnerability

Do not publish sensitive vulnerability details in a public issue. Use the repository owner's private security reporting channel when available.

Include:

- Affected commit or version.
- Reproduction steps.
- Expected and actual behavior.
- Security impact.
- A minimal pose JSON / template file when import parsing is involved.

## Trust model

Pose Studio is a static browser application with no backend.

Its primary protections are:

- Camera frames are transient inference input and are not recorded by the application.
- Recordings contain pose landmarks and timestamps, not source video.
- Recordings and references are stored in local IndexedDB.
- Runtime JavaScript, WASM, and the pose model are embedded into the generated HTML.
- Third-party package versions are pinned exactly.
- The Pose Landmarker model is pinned to an exact HTTPS build source and SHA-256.
- `connect-src blob:` is the only runtime connection permission required by the main application. `script-src 'wasm-unsafe-eval'` is permitted only for the embedded MediaPipe WebAssembly runtime. HTTP and HTTPS runtime connections are blocked by CSP.
- No analytics, telemetry, remote fonts, account system, cloud storage, silent update check, or upload API is included.
- Downloads happen only after explicit user action.

The GitHub Pages deployment still requires the initial HTML request. Browser extensions, the operating system, downloaded-file destinations, and any site where a user later uploads an exported file are outside the application's trust boundary.

A generated HTML file is executable code. Distribute standalone HTML files through a trusted channel and verify hashes in high-trust workflows.

Pose landmark data may still be personal or sensitive in context because it describes human motion. Do not automatically treat `.pose.json`, `.pose-template.json`, or exported viewer HTML as anonymous data.

## Camera permissions

Camera permission is controlled by the browser and operating system.

Pose Studio:

- Requests video only; microphone audio is not requested.
- Starts the camera only after an explicit user action.
- Stops every active video track when the camera is stopped.
- Releases the current `MediaStream` when the page is torn down.
- Does not persist camera pixels to IndexedDB or export them.

## Imported data

`.pose.json` and `.pose-template.json` files are untrusted local input.

The application should:

- Reject imports larger than the configured limit (25 MB in v1.0).
- Require the Browser Kitty pose format marker and supported schema version.
- Accept only the expected recording / reference kinds.
- Parse imported content as JSON data and never execute it as script.
- Avoid unbounded recursion or allocation while validating malformed input.
- Show recoverable user-facing errors rather than raw stack traces.

## Resource handling

Pose Studio should release or bound browser resources where practical:

- Stop camera tracks when not needed.
- Avoid queueing pose-inference frames without bound.
- Invalidate stale asynchronous results when the camera generation changes.
- Revoke temporary Blob URLs.
- Terminate / replace workers when the runtime is restarted.
- Keep recording duration bounded (five minutes in v1.0).

## Dependency review

Before adding or upgrading a package or model:

- Confirm package / artifact identity and exact version.
- Review its license and required redistribution notices.
- Inspect browser-facing bundle code and package scripts.
- Confirm every runtime support asset is embedded into the generated HTML.
- Pin remote build assets with SHA-256.
- Rebuild with a clean cache.
- Test with the runtime network disabled.
- Re-check Content Security Policy and the browser Network panel.

See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for the currently bundled dependencies.

## Motion Notes 2.0 changes

Timed notes are treated as untrusted text: escaped in app markup and assigned through textContent in standalone viewers. JSON imports enforce sample/coordinate/time/note bounds before persistence. PNG rendering does not execute note text. Export filenames strip path separators and control characters.

The embedded MediaPipe JavaScript sender is disabled by an exact, guarded build transform after original npm archive integrity verification. CSP still permits only blob: connections for embedded WASM, blocking HTTP/HTTPS. No telemetry endpoint is contacted by the tested inference flow. Preserve the transform and network tests on upgrades.
