# Template migration — 2026-10-08

Source app: supplied htmlapps-pose-studio-v1.1.0.zip.
Upstream: https://github.com/ttomohisa/htmlapps-template
Pinned main: cb908779682fa315ccd0f1eb58549f6c208f36f0 (checked during this task).

Adopted current AGENTS, components, architecture/component/dependency workflow documentation, Actions workflows (including optional Cloudflare PR preview), preview config, batch preflight, dependency tooling, canonical-icon verification and self-extract builder/verifier. The builder now sources both icons from assets/favicon.svg and generates the repository-root pose-studio.html byte-identically to dist/index.html. Repository identity is intentionally unchanged; Motion Notes is the new product title.

App-specific extensions retained:
- Locked remote pose model with SHA-256 verification.
- Existing npm SRI integrity lock support; upstream tooling can produce SHA-256 locks for future intentional upgrades. Original archive must pass its existing lock before extraction/transform.
- CSP connect-src blob: permits in-memory WASM assets and blocks HTTP/HTTPS, and script-src wasm-unsafe-eval is necessary for inference. The verifier accepts only none or blob:, not arbitrary connect hosts.
- MediaPipe @mediapipe/tasks-vision 1.0.1 contains a periodic telemetry sender. A fail-closed, exact build transform replaces its sender construction (`this.l=new Fh(r)`) with a disabled sink. The timer and sender are never instantiated. Original third-party code remains pinned/verified; the manifest asset hash describes the transformed bytes. Dependency upgrades fail if the transform does not match exactly once. See THIRD_PARTY_NOTICES.md.

No upstream template repository was modified. No remote application repository or deployment was changed.
