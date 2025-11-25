# Tasks

> Development plan — Loom-like Screen Recorder Extension
>
> Follow this checklist and use the checkboxes as your single source of truth. Treat this like code: small commits, clear PRs, and merciless tests.

---

## How to use this file

* Assign tasks to a developer (Dev A / Dev B or names).
* Use the `Priority` field to decide what to ship first (High / Medium / Low).
* Complexity: Low / Medium / High — helps with planning.
* Update status by checking boxes and adding short PR links.

---

## Table of contents

1. Project overview
2. Tech stack & infra decisions
3. Repo structure & conventions
4. Phase 0 — Project setup
5. Phase 1 — MVP (core recording + sharing)
6. Phase 2 — Growth features (AI, editing, analytics)
7. Phase 3 — Scale & SDK (embeds, enterprise)
8. QA / Testing / Security
9. Release / Distribution
10. Post-release & monitoring
11. Optional extras / future wishlist

---

## 1. Project overview

Build a browser extension (Chrome first, then Firefox/Safari) that records screen + webcam + mic, provides lightweight in-browser editing, and offers shareable links (optionally with backend storage). Aim for a smooth UX and efficient resource usage. Start with an MVP that can record, trim, download, and share.

---

## 2. Tech stack & infra decisions

* **Frontend / Extension UI:** TypeScript + React (WXT framework for extension UI)
* **Styling:** TailwindCSS
* **Recorder:** MediaDevices getDisplayMedia + MediaRecorder API (with optional WebCodecs/WebAssembly encoder later)
* **WebExtension APIs:** Chrome Manifest V3 (service worker background), browser.action popup, content scripts
* **Backend (optional):** Supabase (Auth + Storage) or your own Node/Next.js API + S3-compatible storage
* **AI services:** OpenAI (Whisper for transcription / GPT for summaries), optional local ML for some features
* **Database:** Supabase/Postgres or Firebase (if needed for users, links, analytics)
* **CI/CD:** GitHub Actions (lint, build, test, package extension)
* **Issue tracking / project board:** GitHub Projects / Jira

Notes:

* Start without backend for MVP if you want local-first; add Supabase when you need shareable cloud links, accounts, analytics.

---

## 3. Repo structure & conventions

Monorepo (pnpm workspace recommended)

```
/ (root)
  /packages
    /extension    # extension UI, popup, options, content scripts
    /shared       # types, utils, design tokens
    /backend      # optional API, webhooks, upload handlers
    /web-player   # optional web app to view hosted videos
  /docs
  .github/workflows
  package.json
  pnpm-workspace.yaml
```

Conventions:

* TypeScript `strict` enabled
* ESLint + Prettier standard config
* Branching: `main` (stable), `dev` (integration), feature branches `feat/<short-name>`
* Commit messages: Conventional Commits

---

## 4. Phase 0 — Project setup (foundations)

### Goals

* Get a working extension skeleton, CI, linting, and a minimal recording proof-of-concept.

### Tasks

* [ ] Create repo + pnpm workspace. `Priority: High` `Complexity: Low` (Owner: Dev A)
* [ ] Set up TypeScript, ESLint, Prettier, TailwindCSS. `Priority: High` `Complexity: Low` (Owner: Dev B)
* [ ] Init extension scaffold using WXT + React. `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Add Manifest v3 service worker background script template. `Priority: High` `Complexity: Medium` (Owner: Dev B)
* [ ] Add sample popup + options pages. `Priority: High` `Complexity: Low` (Owner: Dev A)
* [ ] CI pipeline: lint, build, test. `Priority: High` `Complexity: Low` (Owner: Dev B)
* [ ] Create PR template and issue templates. `Priority: Medium` `Complexity: Low` (Owner: Dev A)

Acceptance criteria:

* `pnpm build` produces an unpacked extension folder that loads in Chrome via `Load unpacked`.

---

## 5. Phase 1 — MVP (core features)

### Goals

Record screen + webcam + mic; basic trimming; save locally or upload; simple sharing.

### Tasks — Recording core

* [ ] Implement screen capture modes: `current tab`, `window`, `entire screen`. `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Implement webcam overlay (movable, resizable camera bubble). `Priority: High` `Complexity: Medium` (Owner: Dev B)
* [ ] Implement microphone selection and audio capture. `Priority: High` `Complexity: Low` (Owner: Dev A)
* [ ] Cursor highlights & click visualization (on/off toggle). `Priority: Medium` `Complexity: Medium` (Owner: Dev B)
* [ ] Pause / Resume recording. `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Recording indicator and countdown timer. `Priority: Medium` `Complexity: Low` (Owner: Dev B)

### Tasks — Storage & Export

* [ ] Save temporary recordings to IndexedDB while recording. `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Export video to WebM (default) and provide MP4 conversion option (on backend or via ffmpeg.wasm later). `Priority: High` `Complexity: High` (Owner: Dev B)
* [ ] Download button (save to local disk). `Priority: High` `Complexity: Low` (Owner: Dev A)
* [ ] Optional: Upload to Supabase storage + generate shareable link. `Priority: Medium` `Complexity: Medium` (Owner: Dev B)

### Tasks — Player & Editor

* [ ] Lightweight trimming UI (trim start/end). `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Playback speed controls and seek bar. `Priority: Medium` `Complexity: Low` (Owner: Dev B)
* [ ] Preview before saving/uploading. `Priority: High` `Complexity: Low` (Owner: Dev A)

### Tasks — UI/UX

* [ ] Popup recording UI with clear affordances. `Priority: High` `Complexity: Low` (Owner: Dev B)
* [ ] Options page: default camera, mic, recording quality, file format preferences. `Priority: High` `Complexity: Low` (Owner: Dev A)
* [ ] Accessibility checks (keyboard nav, ARIA labels). `Priority: Medium` `Complexity: Medium` (Owner: Dev B)

### Tasks — Basic analytics & privacy

* [ ] Allow user to blur selected areas (basic redaction) during or after recording. `Priority: Medium` `Complexity: Medium` (Owner: Dev A)
* [ ] Add privacy notice & permissions explanation in options page. `Priority: High` `Complexity: Low` (Owner: Dev B)

Acceptance criteria:

* Users can record screen + cam + mic, preview, trim, and download or upload a file that plays back correctly in a browser.

---

## 6. Phase 2 — Growth features (AI, editing, collaboration)

### Goals

Add features that increase stickiness: AI transcripts, auto chapters, comments, and richer editing tools.

### Tasks — AI & Transcription

* [ ] Integrate Whisper/OpenAI for transcription (post-recording). `Priority: High` `Complexity: Medium` (Owner: Dev B)
* [ ] Auto-generate chapters & summaries using GPT. `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Auto-generate video title & thumbnail suggestions. `Priority: Medium` `Complexity: Low` (Owner: Dev B)

### Tasks — Editing & UX

* [ ] Add annotations: text boxes, arrows, highlights (post-recording editor). `Priority: High` `Complexity: High` (Owner: Dev A)
* [ ] Allow re-recording of segments and stitching (branching takes). `Priority: Medium` `Complexity: High` (Owner: Dev B)
* [ ] Translated captions (multi-language). `Priority: Medium` `Complexity: Medium` (Owner: Dev A)

### Tasks — Collaboration

* [ ] Time-coded comments / replies on the player. `Priority: High` `Complexity: Medium` (Owner: Dev B)
* [ ] Reaction emojis + threaded replies. `Priority: Medium` `Complexity: Medium` (Owner: Dev A)
* [ ] Integrations: Slack / Microsoft Teams share (deep link preview). `Priority: Low` `Complexity: Low` (Owner: Dev B)

### Tasks — Analytics

* [ ] Viewer engagement heatmaps (rewind/skip hotspots). `Priority: Medium` `Complexity: High` (Owner: Dev A)
* [ ] Basic metrics dashboard (views, average watch time). `Priority: Medium` `Complexity: Medium` (Owner: Dev B)

Acceptance criteria:

* Transcripts + chapters are available after recording; users can annotate and comment on videos; basic analytics populate on views.

---

## 7. Phase 3 — Scale, SDK & enterprise features

### Goals

Provide embeddable SDK, enterprise-grade privacy and storage options, live streaming, and team management.

### Tasks — SDK & Embedding

* [ ] Create an embeddable recorder SDK for websites (configurable UI, hooks). `Priority: High` `Complexity: High` (Owner: Dev A)
* [ ] Document SDK with examples and demos. `Priority: High` `Complexity: Medium` (Owner: Dev B)

### Tasks — Enterprise features

* [ ] Team accounts, roles, and permissions. `Priority: High` `Complexity: High` (Owner: Dev B)
* [ ] SSO integrations (SAML / OIDC). `Priority: Medium` `Complexity: High` (Owner: Dev A)
* [ ] On-prem or private S3 storage support. `Priority: Medium` `Complexity: Medium` (Owner: Dev B)

### Tasks — Live / Streaming

* [ ] Live streaming support (WebRTC-based) + live comments. `Priority: Medium` `Complexity: High` (Owner: Dev A)
* [ ] Record live sessions for VOD later. `Priority: Medium` `Complexity: High` (Owner: Dev B)

### Tasks — Advanced security

* [ ] Video link expiry, password protection, IP restrictions. `Priority: High` `Complexity: Medium` (Owner: Dev A)
* [ ] Audit logs for enterprise accounts. `Priority: Medium` `Complexity: Medium` (Owner: Dev B)

Acceptance criteria:

* SDK is stable, documented, and used in a demo site; enterprise features behind team billing and access controls.

---

## 8. QA / Testing / Security

* [ ] Unit tests for core utilities (Jest / vitest). `Priority: High` `Complexity: Low`
* [ ] E2E tests for recording flow (Playwright). `Priority: High` `Complexity: High`
* [ ] Performance tests — long recording stability, memory and CPU profiling. `Priority: High` `Complexity: High`
* [ ] Security review for permissions and data storage (GDPR, CCPA). `Priority: High` `Complexity: High`
* [ ] Penetration testing for backend APIs (if any). `Priority: Medium` `Complexity: High`

Testing checklist:

* Test on Chrome desktop and Chrome-based Edge, Firefox, macOS and Windows. Mobile later.
* Validate MediaRecorder behavior across browsers and fallback gracefully.

---

## 9. Release & Distribution

* [ ] Pack extension for Chrome Web Store (Manifest v3). `Priority: High` `Complexity: Medium`
* [ ] Create store listing assets and privacy policy. `Priority: High` `Complexity: Low`
* [ ] Prepare Firefox AMO build. `Priority: Medium` `Complexity: Medium`
* [ ] Optional: Safari extension (requires porting to Safari Web Extensions). `Priority: Low` `Complexity: High`

Release checklist:

* Automated build and signing pipeline
* Store screenshots and marketing blurb
* Legal: privacy policy, terms of service, data retention policy

---

## 10. Post-release & monitoring

* [ ] Set up error monitoring (Sentry) for extension & backend. `Priority: High` `Complexity: Low`
* [ ] User feedback channel built into options page (send logs + notes). `Priority: Medium` `Complexity: Low`
* [ ] Monitor usage and performance metrics. `Priority: High` `Complexity: Medium`
* [ ] Plan cadence for bugfix/feature releases (e.g., weekly or biweekly sprint). `Priority: Medium` `Complexity: Low`

---

## 11. Optional extras / wishlist (future)

* Smart auto-zoom & focus on clicks
* Gesture-driven annotation via webcam
* Auto-translated captions and multi-language UI
* AI-driven “workflow suggestions” (detect repeated clicks and suggest shortcuts)
* Scroll-capture screenshots & scrolling video capture
* In-browser high fidelity editor (timeline, transitions)

---

## Definitions of Done (DoD)

1. Code merged to `main` with passing CI.
2. Unit & E2E tests added for the feature, or a test plan documented.
3. Feature documented in `docs/` and options page updated where relevant.
4. Accessibility audit performed for visible UI.

---

## Recommended milestones (suggested order of delivery)

1. Project setup & recording PoC
2. MVP recording + local download + trimming
3. Upload + cloud sharing + basic player
4. Transcription + auto chapters
5. Comments + collaboration
6. Embeddable SDK
7. Enterprise features & scaling

---

## Final notes

* Keep privacy and explicit permission flows tight. Screen recording is sensitive; users must *trust* your extension.
* Start small. Ship a solid recording + trim + download flow before chasing AI magic.

---

If you want, I can convert this into a set of ready-to-import GitHub issues (with labels and checklists). Or scaffold the WXT extension code and a working MediaRecorder PoC. Which one do you want first?
