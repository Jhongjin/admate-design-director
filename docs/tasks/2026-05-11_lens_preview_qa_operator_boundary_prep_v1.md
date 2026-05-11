# Lens Preview QA Operator Boundary Prep v1

Date: 2026-05-11
Gate: Lens-Preview-QA-Operator-Boundary-Prep
Status: docs-only prep packet
Depends on:
- `docs/tasks/2026-05-11_design_director_lens_preview_qa_1_operator_boundary_checklist_v1.md`
- `docs/product-ui-direction/lens-preview-ux-direction-v1.md`
- `docs/product-ui-direction/lens-ui-direction-v1.md`
- `docs/prompts/lens-ui-review-prompt-v1.md`

Scope: prepare a read-only Lens Product Agent QA handoff for the later
Lens-Preview-QA-Operator-Boundary gate. This packet is instruction-only and
does not authorize product repo edits, capture execution, screenshot creation,
browser/session work, rendering/capture engine/composite/injection changes,
DB/API/env/storage work, or golden PNG/asset changes.

## Purpose

Lens preview QA must protect output fidelity while giving operators enough
context to verify status, metadata, diagnostics, and safe review actions.

Design Director position:

```text
Capture output bitmap is evidence.
Preview UI may help inspect it, but must not reinterpret, decorate, or mutate it.
```

This prep packet exists so a Lens Product Agent can perform a read-only review
using already available screenshots or approved evidence, then report clear
P0/P1 findings without changing the product or generating new capture evidence.

## Read-Only Lens Product Agent Prompt

Use the following prompt for the Lens Product Agent:

```text
You are the AdMate Lens Product Agent.

Gate:
Lens-Preview-QA-Operator-Boundary

Mode:
Read-only QA review. You may inspect existing documentation and already
provided evidence only. You must not change files, run product flows, start a
browser session, create screenshots, execute captures, upload files, call
production services, or mutate any product state.

Goal:
Review the Lens preview workspace and operator boundary for output fidelity
protection, inspector metadata clarity, diagnostics clarity, safe action
hierarchy, mobile layout stability, and status-state separation.

Primary rule:
The capture output bitmap is evidence. The preview workspace may display and
inspect it, but must not crop, filter, recolor, overlay, watermark, decorate,
reinterpret, regenerate, or otherwise change the output image.

Hard no-touch rules:
- Do not edit the product repo.
- Do not modify product code, tests, fixtures, generated files, lockfiles, or
  config.
- Do not execute captures.
- Do not create screenshots, recordings, image files, generated media, or
  visual assets.
- Do not start or attach to a browser, logged-in session, local dev server, or
  remote dashboard.
- Do not change rendering, capture engine, synthetic rendering, composite,
  injection, platform template, placement fidelity, or preview fidelity logic.
- Do not change API contracts, DB/schema, auth/session/permission, environment
  variables, storage policy, storage objects, queues, jobs, or external
  service configuration.
- Do not change golden PNGs, golden samples, fixtures, product assets, or
  committed screenshots.
- Do not upload, delete, approve, retry, regenerate, cancel, or otherwise
  mutate product data.
- Do not expose signed URLs, tokens, cookies, credentials, raw provider
  payloads, private dashboard paths, real campaign/account identifiers, or
  unapproved production screenshots.

Required review surfaces from existing evidence:
- completed preview desktop
- completed preview mobile
- capture request list
- completed capture detail
- review-needed capture detail if already available
- queued or processing state if already available
- failed or unavailable image state if already available
- cancel control visibility if already shown in existing evidence
- download/copy/open/zoom action hierarchy
- inspector metadata and diagnostics visibility
- confirmation that output bitmap and capture logic are unchanged

Report format:
- Evidence inventory: list the existing screenshots/evidence reviewed, or state
  why an item was unavailable.
- Redaction check: confirm no sensitive value appears in evidence or notes.
- P0 blockers: only issues that break output fidelity, expose secrets, blur
  critical states, or make mobile review unusable.
- P1 follow-ups: polish or clarity issues that do not alter evidence fidelity.
- Pass/fail call: pass, pass with P1 follow-ups, or fail with P0 blockers.
- No-touch confirmation: explicitly confirm no product repo edits, capture
  execution, screenshot creation, browser/session work, rendering/capture
  engine/composite/injection changes, DB/API/env/storage work, or golden
  PNG/asset changes were performed.
```

## Required Screenshots And Evidence List

The future review should use only evidence that already exists or was
separately approved and supplied before the gate starts.

Required if available:

- desktop completed preview screenshot
- mobile completed preview screenshot or supplied viewport capture
- capture request list screenshot
- completed capture detail screenshot
- review-needed capture detail screenshot
- queued or processing state screenshot
- failed capture screenshot
- unavailable image state screenshot
- cancel control screenshot for an already-running or previously documented
  running state
- action hierarchy screenshot showing download, copy URL, copy storage path,
  open original, and zoom controls
- inspector screenshot showing status, placement, surface/device, created time,
  capture time, elapsed duration, result category, landing availability,
  hostname or URL summary, storage path summary, diagnostics summary, and
  quality flags
- sanitized metadata sample with sensitive fields redacted or summarized
- written no-touch confirmation covering product repo edits, capture execution,
  screenshot creation, browser/session work, rendering/capture
  engine/composite/injection changes, DB/API/env/storage work, and golden
  PNG/asset changes

Unavailable evidence is acceptable when the agent clearly records the gap. The
agent must not create missing screenshots or run product flows to fill the gap
inside this gate.

## Redaction Rules

Redact before sharing evidence or notes:

- signed URLs, presigned query strings, bearer/session tokens, cookies, API
  keys, credentials, private keys, secrets, and authorization headers
- raw provider payloads, request/response bodies, stack traces with private
  paths, and private dashboard URLs
- real campaign names, advertiser names, account IDs, customer IDs, user email
  addresses, phone numbers, and billing identifiers
- storage object paths or URLs that include private account, tenant, campaign,
  or token-like segments
- production screenshots that were not approved for QA evidence

Allowed summaries:

- hostname instead of full URL
- storage bucket/category summary instead of full object path
- timestamp rounded to the useful review granularity
- placement/platform labels when they do not identify a real advertiser or
  account
- diagnostic category and count without raw payload detail

If a value looks secret-like, private, or production-identifying, the agent must
stop quoting it and report only the field name plus redaction reason.

## Pass/Fail Boundaries

### Pass

The gate passes when existing evidence shows:

- output image is displayed at original ratio in a neutral viewer
- preview frame, badges, toolbar, inspector, and actions are clearly outside
  the output bitmap
- metadata and diagnostics do not cover the output
- completed, review-needed, failed, queued, processing, unavailable, and
  cancelled states are visually distinct when present
- diagnostics do not claim pixel match, golden verification, or production
  fidelity without measured evidence
- safe actions are grouped separately from risky or state-changing actions
- mobile preview keeps the image primary and prevents action/text overlap
- no sensitive values are visible in screenshots, notes, or metadata samples
- no product repo edits or product-state mutations occurred

### Pass With P1 Follow-Ups

The gate may pass with P1 follow-ups when the output remains protected and no
sensitive data is exposed, but evidence shows:

- inspector field order needs polish
- storage path wrapping or copy feedback needs improvement
- review-needed reason is too long or unclear
- cancel copy should clarify item vs batch behavior
- mobile action labels need icon/accessibility treatment
- duplicate publisher entries need clearer request-level labeling
- missing evidence limits confidence but does not hide a known P0 blocker

### Fail

The gate fails with a P0 blocker if any of the following appears:

- output bitmap is cropped, filtered, recolored, overlaid, watermarked,
  decorated, or visually changed by the preview UI
- preview frame or operator controls appear to be part of the captured image
- metadata, diagnostics, toolbar, sticky action bar, or inspector covers the
  output in a way that changes reviewability
- signed URL, token, cookie, credential, raw provider payload, secret-like
  value, private dashboard path, real campaign/account identifier, or
  unapproved production screenshot is exposed
- completed, review-needed, failed, queued/processing, unavailable, or
  cancelled states are indistinguishable in a way that could mislead operators
- diagnostics claim pixel fidelity, golden sample verification, or production
  fidelity without measured QA evidence
- mobile preview hides the output or primary safe actions
- destructive or state-changing actions are mixed with safe review actions
  without separation or confirmation
- the Product Agent edits the product repo, executes captures, creates
  screenshots, starts browser/session work, changes rendering/capture
  engine/composite/injection logic, touches DB/API/env/storage, or modifies
  golden PNGs/assets

## No-Touch Rules

This prep packet explicitly forbids:

- product repo edits
- capture execution
- screenshot creation
- recording creation
- browser work
- session work
- local dev server work
- remote dashboard work
- rendering engine changes
- capture engine changes
- synthetic rendering changes
- composite logic changes
- injection logic changes
- platform template changes
- placement fidelity changes
- preview fidelity changes
- DB work
- API work
- auth/session/permission work
- environment variable work
- storage policy work
- storage object work
- queue/job mutation
- upload, delete, approve, retry, regenerate, cancel, or cleanup execution
- golden PNG changes
- golden sample changes
- fixture image changes
- committed screenshot changes
- product asset changes
- secret, token, credential, cookie, signed URL, raw payload, private URL, real
  campaign/account, or unapproved production screenshot exposure

## Evidence Review Checklist

| Area | Pass expectation | Fail signal |
| --- | --- | --- |
| Output image | Original ratio, neutral viewer, no mutation. | Crop, filter, recolor, overlay, watermark, decoration, or visual reinterpretation. |
| Preview boundary | Controls live outside the bitmap. | Frame, badge, inspector, or toolbar reads as captured content. |
| Inspector | Metadata and diagnostics are beside/below output. | Inspector covers output or hides evidence content. |
| Status | Completed, review-needed, failed, processing, queued, unavailable, and cancelled are distinct. | Risk states look like completed or recoverable without reason. |
| Diagnostics | Summary explains review/failure without fidelity overclaim. | "Pixel match passed", "golden verified", or equivalent appears without measured evidence. |
| Actions | Download, copy, open, and zoom are grouped as safe review actions. | Retry, regenerate, approve, delete, or cancel is mixed with safe actions without separation. |
| Sensitive data | Values are redacted, summarized, wrapped, or omitted. | Secret-like values, signed URLs, raw payloads, or private identifiers are visible. |
| Mobile | Image remains primary; accordions/actions do not overlap. | Sticky bars, long text, or controls cover output or create accidental tap risk. |

## Product Agent Output Template

```text
Gate:
Lens-Preview-QA-Operator-Boundary

Evidence reviewed:
- [item]: [provided / unavailable / redacted]

Redaction check:
- [pass/fail]
- Sensitive values exposed: [none / field names only]

P0 blockers:
- [none or issue list]

P1 follow-ups:
- [none or issue list]

Pass/fail call:
- [pass / pass with P1 follow-ups / fail]

No-touch confirmation:
- No product repo edits.
- No capture execution.
- No screenshot or recording creation.
- No browser/session/dev-server/dashboard work.
- No rendering, capture engine, synthetic rendering, composite, injection,
  platform template, placement fidelity, or preview fidelity changes.
- No DB, API, auth/session/permission, env, storage, queue, or job mutation.
- No golden PNG, golden sample, fixture image, committed screenshot, or product
  asset changes.
- No secret, token, credential, cookie, signed URL, raw payload, private URL,
  real campaign/account, or unapproved production screenshot exposure.
```

## Design Director Handoff Note

This is a prep gate only. It should help the future Lens Product Agent perform
read-only QA and produce a clear boundary report. Any request to implement UI
changes, run captures, create screenshots, inspect live sessions, adjust
capture/rendering behavior, touch storage/API/DB/env, or update golden assets
must be moved to a separately approved gate.
