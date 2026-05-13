# Design Director Lens Cockpit Foundation Plan v1

Date: 2026-05-13
Gate: Design-Director-Lens-Cockpit-Foundation-1
Status: Draft, docs-only
Owner: Design Director / Commander review

## Scope

This document defines the Lens cockpit foundation for future product implementation.

Allowed:

- Translate the AdMate taste strategy into Lens-specific UX direction.
- Define capture queue, detail viewer, QA, golden sample, and operator controls.
- Prepare a handoff-ready plan for Lens product agents.

Not allowed:

- Lens repo code changes.
- Capture job execution.
- External asset/template/vendor import.
- Production API calls, SQL, DB/Auth mutation, n8n execution.
- Secret, token, cookie, session, credential, or environment value readback.

## Product Role

Lens is the visual proof system for AdMate.

It should feel like a production-grade capture and QA cockpit: operators can see what is queued, what is running, what succeeded, what needs review, and why a visual result can be trusted.

## Audience

Primary:

- Capture operators.
- QA reviewers.
- Creative and performance teams validating media evidence.

Secondary:

- Product owners checking capture reliability.

## First Viewport Contract

The Lens first viewport must answer:

1. What capture work is running or waiting?
2. Which outputs are ready, failed, blocked, or need review?
3. Which item should the operator inspect next?
4. What visual evidence supports the status?

Required modules:

- Queue health summary.
- Active capture lane with cancel/stop affordance when supported.
- Review-needed lane.
- Recent successful captures with thumbnail and status.
- Capture detail preview entry.
- Runtime/provider state where relevant.

## Capture Queue Contract

Queue rows should expose:

- Channel and surface.
- Placement URL or sanitized host label.
- Creative/asset status.
- Job state: queued, running, success, failed, review-needed, canceled.
- Duration and retry state.
- Operator-safe action: view, retry, cancel, mark reviewed.

Long-running items need clear elapsed time and stop/cancel affordance if the backend supports it. If stop is not yet implemented, UI should say “stop not available” rather than pretending control exists.

## Detail Viewer Contract

The detail viewer should prioritize visual trust:

- Large screenshot viewport.
- Stable zoom controls.
- Side panel with sanitized capture metadata.
- QA flags and reason.
- Original/open/download buttons only where safe.
- Golden candidate marker.

The viewer should not over-crop or hide the actual placement context. It should allow the reviewer to understand whether the ad is visible, proportional, and plausibly placed.

## Golden Sample Guidance

Golden samples should be sanitized and product-owned.

Preferred:

- Generic or controlled capture-like fixtures.
- No real third-party brand if used for committed repo assets.
- No raw user/session/campaign identifiers.
- Stable dimensions and deterministic visual checks.

Allowed for human review only:

- Temporary screenshots from real media environments, if not committed as product assets.

Not recommended:

- Committing raw third-party publisher or YouTube screenshots as reusable golden assets.

## Visual Taste Direction

Lens should feel like:

- Visual production queue.
- QA light table.
- Capture control room.

It should not feel like:

- Marketing gallery.
- File manager.
- Generic admin table.
- Decorative portfolio page.

Use visual density thoughtfully. Thumbnails should carry the experience, but metadata and state must remain scannable.

## Responsive Contract

Desktop:

- Queue and detail can sit side by side.
- Active item and preview should remain visible.
- QA panel should not crowd the screenshot.

Mobile:

- Queue first, detail second.
- Screenshot viewer uses stable zoom and avoids horizontal overflow.
- Actions collapse into clear controls.

## Empty, Failed, And Degraded States

Empty queue:

- Show how capture work enters the queue.
- Do not imply a failure when no work exists.

Failed capture:

- Show safe reason category.
- Keep raw browser logs hidden by default.
- Offer retry only if safe.

Degraded provider:

- Show provider/runtime issue.
- Preserve completed captures.
- Avoid losing pending job context.

## Implementation Candidate Surfaces

Future implementation gate may inspect and update:

- Capture queue page.
- Capture detail viewer.
- QA/golden sample metadata model.
- Cancel/stop control surface if backend support exists.
- Responsive viewer layout.

Capture engine, storage, SQL, and production execution require separate explicit approval.

## Verification Plan

- Desktop capture queue screenshot.
- Mobile capture queue screenshot.
- Detail viewer screenshot.
- No horizontal overflow.
- Long-running job state remains readable.
- Review-needed and success states are visually distinct.
- No raw secrets, raw sessions, raw provider payloads, or private identifiers.

## Acceptance Criteria

- Lens feels like an operator cockpit for visual proof.
- The next capture/review action is obvious.
- Golden sample policy is safe and future-proof.
- Capture detail gives enough visual context to trust or reject a placement.
