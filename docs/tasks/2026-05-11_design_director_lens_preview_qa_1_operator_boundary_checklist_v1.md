# Design Director Lens Preview QA 1 Operator Boundary Checklist v1

Date: 2026-05-11
Gate: Design-Lens-Preview-QA-1
Status: docs-only gate
Depends on:
- `docs/product-ui-direction/lens-ui-direction-v1.md`
- `docs/product-ui-direction/lens-preview-ux-direction-v1.md`
- `docs/prompts/lens-ui-review-prompt-v1.md`

Scope: define a Lens preview and operator-boundary QA checklist for a future
product review gate. This gate does not change Lens code, capture output,
rendering, injection, composite logic, API, DB, environment, screenshots,
assets, or product repo files.

## Purpose

Lens preview QA must protect the capture output while improving operator
confidence around status, metadata, diagnostics, and safe review actions.

The Design Director position is:

```text
Capture output bitmap is evidence.
Preview UI may help inspect it, but must not reinterpret or decorate it.
```

Recent Lens work introduced stronger operator controls and GDN review evidence.
The next design QA should therefore focus on the boundary between the immutable
output image and the surrounding review workspace.

## Review Inputs For Future Gate

A future Lens product Agent should submit:

- desktop preview screenshot or screen recording
- mobile preview screenshot or viewport capture
- capture request list screenshot
- completed capture detail screenshot
- review-needed capture detail screenshot when available
- processing or queued state screenshot when available
- failed or unavailable image state screenshot when available
- sanitized metadata sample with storage path, URL, diagnostics, duration, and
  result category redacted or summarized as needed
- no-touch confirmation for capture engine, rendering, composite, injection,
  storage policy, DB/schema/env, and golden PNG/product assets

Do not submit signed URLs, tokens, cookies, credentials, raw provider payloads,
private dashboard paths, real campaign/account identifiers, or unapproved
production screenshots.

## Boundary Checklist

| Area | Pass expectation | Failure signal |
| --- | --- | --- |
| Output image | Image is displayed at original ratio in a neutral viewer. | Output is cropped, filtered, recolored, watermarked, overlaid, or visually themed by AdMate UI. |
| Preview frame | Viewer background and controls are outside the output bitmap. | Frame, badge, inspector, or action UI appears to be part of the captured image. |
| Inspector | Metadata and diagnostics sit beside or below the image, not on top of it. | Metadata, buttons, or diagnostics cover the output. |
| Status | Completed, review needed, failed, processing, unavailable are visually distinct. | Review-needed looks like pass, failed looks recoverable without reason, or queued/running lacks progress context. |
| Diagnostics | Warnings summarize why review is needed without claiming pixel fidelity. | Diagnostics imply "pixel match passed" or "golden verified" without measured evidence. |
| Safe actions | Download, copy URL, copy path, open original, zoom are grouped as review actions. | Destructive or regenerating actions are mixed with safe review actions. |
| Sensitive fields | Storage/path/URL values are summarized, wrapped, and copyable without exposing secrets. | Signed URLs, tokens, credentials, raw provider data, or private IDs are visible. |
| Mobile | Image remains primary, metadata collapses into accordions, sticky actions do not cover the image. | Source controls push image off-screen, overlap, or create accidental tap risk. |

## State Matrix

| State | Operator expectation | Design QA question |
| --- | --- | --- |
| `queued` | Request is accepted but not actively capturing. | Does the list make queued vs processing clear without implying a stuck job? |
| `processing` | Capture is actively running. | Is there an obvious way to understand current item and elapsed time? |
| `completed` | Output exists and can be reviewed. | Is the image large enough and are safe actions available without covering it? |
| `review-needed` | Output exists but needs human inspection. | Is the reason visible, concise, and not framed as a pass? |
| `failed` | Output did not complete. | Are failure reason, retryability, and affected placement separated? |
| `unavailable` | Metadata may exist but image or URL is missing. | Does the UI preserve metadata while clearly disabling invalid actions? |
| `cancelled` | Capture was intentionally stopped. | Does it read as operator action rather than system failure? |

## Recent Lens Context To Preserve

Future QA should account for current operating realities:

- GDN captures can run as a batch and some publisher pages can be slow.
- Operator cancel control is important for long-running captures.
- Duplicate or repeated publisher entries should be visible to the operator as
  request data, but design QA should not change dedupe/capture logic.
- External-only golden approval can exist without committing product PNG assets.
- Review-needed output can be a valid completed capture when diagnostics ask
  for human inspection.

These are review conditions, not approval to edit engine behavior.

## Inspector Field Checklist

Required or recommended inspector concepts:

- status badge
- channel and placement
- surface/device
- created time
- capture time
- elapsed duration
- result category
- landing availability
- source URL or hostname summary
- storage path summary
- diagnostics summary
- quality flags or review-needed reason
- safe actions: download, copy URL, copy path, open original

Display rules:

- Use hostname or summarized labels when full URLs contain private paths or
  query strings.
- Use wrapped monospace for storage paths only when the value is approved for
  operator display.
- Disable unavailable actions with a reason.
- Never show signed URLs or raw provider responses in screenshots or docs.
- Do not convert diagnostics into a fidelity guarantee.

## Action Hierarchy

Safe review actions:

- fit
- zoom 100/150/200
- open original
- download
- copy URL
- copy storage path

Risky or state-changing actions should be separated:

- retry
- regenerate
- approve
- delete
- cancel

Cancel-specific design requirements:

- cancel must be reachable while a batch item is running
- cancel copy should make clear whether it stops the current item, queued items,
  or the entire batch
- cancelled state should preserve audit/provenance metadata
- cancel should not delete storage objects or completed rows unless a separate
  product gate explicitly allows cleanup behavior

## Mobile Review Checklist

Mobile pass criteria:

- the image remains the primary visual element
- top actions do not wrap into the image
- metadata and diagnostics use accordion or detail sections
- sticky action bar does not cover key output content
- button labels fit or use icons with accessible labels
- storage path and URL text do not overflow
- image unavailable state still shows metadata

Mobile blockers:

- action bar hides output
- inspector covers output
- copy/open/download buttons overlap
- long Korean diagnostics text overflows
- storage path creates horizontal scroll
- destructive action is too close to safe review actions

## P0/P1 Classification

P0 blockers:

- output bitmap is altered or visually decorated
- signed URL, token, cookie, credential, raw provider response, or secret-like
  value appears in UI or QA evidence
- failed/review-needed/cancelled states are indistinguishable from completed
- mobile preview hides the output or primary action area
- diagnostics claim golden or pixel fidelity without measured QA evidence

P1 follow-ups:

- inspector field ordering needs improvement
- storage path wrapping or copy feedback needs polish
- cancel copy should clarify item vs batch behavior
- review-needed reason should be more concise
- mobile action labels need icon/accessibility treatment
- duplicate publisher entries need clearer request-level labeling

## Lens Product Agent Prompt

Use this prompt for the next Lens product QA handoff:

```text
You are the AdMate Lens product Agent.

Gate:
Lens-Preview-QA-Operator-Boundary

Goal:
Run read-only UI QA of the Lens preview workspace and operator boundary.
Focus on image fidelity protection, inspector metadata, diagnostics, safe
actions, mobile layout, and cancel/review-needed state clarity.

Do not modify code in this Gate.
Do not run new captures unless separately approved.
Do not upload files.
Do not change capture engine, rendering, injection, composite, storage policy,
DB/schema/env, auth, API contracts, golden PNGs, or product assets.
Do not expose signed URLs, tokens, cookies, credentials, raw provider payloads,
or private identifiers.

Required checks:
- completed preview desktop
- completed preview mobile
- review-needed state if available
- processing or queued state if available
- failed or unavailable image state if available
- cancel control visibility if a running capture is available
- download/copy/open/zoom action hierarchy
- inspector metadata and diagnostics visibility
- confirmation that output bitmap is unchanged

Report:
- screenshots used or reason screenshots were unavailable
- P0 blockers
- P1 follow-ups
- no-touch confirmation
- sensitive value exposure check
- next implementation gate recommendation
```

## No-Touch Confirmation

This gate does not perform:

- Lens product code changes
- capture execution
- upload execution
- screenshot capture
- image, video, render, generated media, or asset creation
- golden PNG or product asset creation/modification
- capture engine, rendering, composite, injection, API, DB, storage policy,
  auth, env, or schema changes
- production API calls
- external repo edits
- secret, env, token, cookie, credential, signed URL, or private URL output

## Next Gate

Recommended next gate:

```text
Gate Lens-Preview-QA-Operator-Boundary
```

Suggested purpose:

```text
Have the Lens product Agent perform read-only preview/operator UI QA using
existing completed/review-needed captures and screenshots. Keep any new capture
execution, product code changes, golden asset creation, and storage cleanup out
of scope unless separately approved.
```
