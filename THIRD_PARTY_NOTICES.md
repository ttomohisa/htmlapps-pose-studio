# Third-Party Notices

Pose Studio bundles the following third-party components into generated standalone HTML releases.

## MediaPipe Tasks Vision

- Package: `@mediapipe/tasks-vision`
- Version: `1.0.1`
- License: Apache License 2.0
- Project: https://ai.google.dev/edge/mediapipe

The JavaScript module and required WebAssembly support files are taken from the exact npm package version declared in `dependencies.json` and embedded at build time.

## MediaPipe Pose Landmarker Lite model

- Artifact: `pose_landmarker_lite.task`
- Variant: float16, version 1
- License: Apache License 2.0
- Project: https://ai.google.dev/edge/mediapipe/solutions/vision/pose_landmarker
- Build-time source: the pinned official `storage.googleapis.com/mediapipe-models/.../pose_landmarker_lite.task` URL declared in `dependencies.json`
- Expected SHA-256: `59929e1d1ee95287735ddd833b19cf4ac46d29bc7afddbbf6753c459690d574a`

The model is downloaded only during the build and is then embedded into the generated HTML. Runtime model download is not used.

## Runtime network note

Pose Studio's application CSP permits `connect-src blob:` only so the embedded runtime can read in-memory Blob URLs for WASM/model initialization. HTTP and HTTPS runtime connections are not permitted. This also prevents third-party runtime code from transmitting metrics to external endpoints.

See the upstream projects for the complete Apache License 2.0 terms and notices. The Pose Studio application code remains licensed under this repository's MIT License.

## Local privacy modification (Motion Notes 2.0.0)

The pinned MediaPipe tasks-vision 1.0.1 JavaScript bundle is modified at build time to disable construction of its periodic telemetry sender. The exact-match transformation is documented in `build-standalone.ps1` and fails on unexpected upstream code. Downloaded archives are checked against the original lock before transformation. Inference and model contents are otherwise unchanged. This modification is distributed under the same Apache-2.0 license and is not an upstream MediaPipe release.
