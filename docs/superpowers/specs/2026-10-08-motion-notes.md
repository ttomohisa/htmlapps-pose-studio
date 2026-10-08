# Motion Notes 2.0 design

User authorization: modernize the supplied Pose Studio v1.1.0 ZIP against current htmlapps-template, preserve core capabilities, radically redesign purpose/title without intermediate approval while the user is away. ZIP is the source of truth; do not publish or merge.

## Decision
Three directions considered: a sports scoring coach (too much implied judgment); a pose-data laboratory (technical and broad); a movement notebook (selected). Primary job: capture movement as landmarks, review moments, add observations, save a portable record without source footage. Name: Motion Notes / 動きのノート. Version 2.0.0. Keep repository name and IndexedDB name for compatibility.

## Main workflow
Three top-level pages on desktop and phone: capture, review, library. Review is the center: large stick-figure player, adjacent seek and playback controls, timestamped notes, editable output filename, JSON/CSV/HTML export and a PNG contact sheet of marked moments. Capture has an obvious video/camera/demo choice. Library opens existing recordings. Secondary analysis/comparison/reference pages remain reachable through explicit review tools. They no longer dominate onboarding. All primary actions have Japanese and English labels.

## Data and privacy
Preserve browser-kitty-pose schemaVersion 1, all existing landmarks/world-landmarks, DB stores and reference formats. Add optional `notes: [{id,t,text}]`, max 24 notes, text max 240. Older records work without notes. Source footage remains transient. Exported JSON, HTML and contact sheet include notes. HTML exports must escape untrusted names/notes. Strict import validation for finite coordinates/timestamps, monotonically increasing samples, 5-minute/20000-sample limits. No interpolation across missing detections or gaps >350ms. Data may be personally identifying; never call anonymous.

## Template migration
Pin template main cb908779682fa315ccd0f1eb58549f6c208f36f0. Adopt generic workflows, components, preflight, favicon placeholder, root HTML copy. Preserve the app-specific locked remote model and npm-integrity build extensions instead of deleting them. Model/runtime dependency versions are unchanged.

## Acceptance
Desktop 1360x900, phone 360x800 and short dialogs; no horizontal overflow. Main note/edit/delete/reload flow. Malformed import rejection with no existing-data loss. Legacy recording/reference roundtrip. User filenames persist across language changes. Deleted last recording disables exports. JSON/CSV/HTML/PNG contents checked. Actual source-frame handling and inference checked where runtime permits; clearly record unverified physical camera/device cases. Both single-HTML variants tested offline. Representative real app screenshots, updated bilingual README and help, template migration note and test report.
