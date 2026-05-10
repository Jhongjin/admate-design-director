# Design Director Compass Evidence QA 1 State Matrix v1

Date: 2026-05-10
Gate: Design-Director-Compass-Evidence-QA-1
Status: docs-only gate
Scope: define the Compass evidence QA state matrix and review boundaries for future implementation work. This gate does not change product code, assets, APIs, screenshots, generated media, or external repos.

## Purpose

Compass evidence QA needs a clear state matrix before any UI, API, or data
contract work proceeds. The matrix below defines the expected evidence states,
source panel behavior, internal field exposure guard, screenshot/evidence
policy, and production API boundary for downstream gates.

## State Matrix

| State | Meaning | Required UI/QA expectation | Evidence/source panel expectation |
| --- | --- | --- | --- |
| `source-found` | A claim, recommendation, or Compass output has at least one acceptable source or evidence record available for review. | Show the output as evidence-supported. The state should not imply source quality beyond what the evidence metadata can prove. | Display source title or label, source type, captured or observed date when available, confidence or review status when available, and a path to inspect the evidence record. |
| `noData` | The system has no usable source or evidence record for the requested claim, recommendation, segment, account, or time range. | Show an explicit no-data state instead of fabricating support. Avoid placeholder metrics, mock proof, or inferred source claims. | Display a concise no-data reason and the checked scope when available. Do not show empty source shells that look like validated evidence. |
| `generation-limited` | The output was generated under incomplete, low-confidence, stale, redacted, or policy-limited evidence conditions. | Show the output as limited. Require copy that makes the limitation visible before the user relies on it. Do not upgrade the state to supported unless acceptable evidence is attached. | Display limitation reason, missing evidence category when known, and any available partial source summary. Keep unsupported generated language separate from verified source facts. |

## Evidence And Source Panel Expectations

The source panel should answer four review questions:

- What evidence record supports this output?
- Where did the source come from?
- When was the evidence captured, observed, or last refreshed?
- What is the current review state?

Expected panel fields:

- evidence state: `source-found`, `noData`, or `generation-limited`
- source label or title when available
- source category such as first-party account data, uploaded reference,
  approved benchmark, policy note, or analyst-authored note
- capture, observation, or refresh date when available
- review status such as unreviewed, accepted, rejected, expired, or limited
- evidence record identifier suitable for internal traceability
- limitation reason when the state is `noData` or `generation-limited`

The panel should separate verified source facts from generated interpretation.
Generated interpretation may cite or summarize evidence, but it should not be
presented as a source of record.

## Internal Field Exposure Guard

Do not expose internal fields in customer-facing UI, screenshots, exports,
shared review packets, or demo material unless a separate approval gate allows
it.

Guarded fields include:

- raw source payloads
- account, tenant, workspace, advertiser, campaign, creative, or audience IDs
- internal prompt, model, trace, job, run, vector, embedding, or retrieval IDs
- confidence internals not designed for customer display
- reviewer identity, internal comments, policy labels, or escalation notes
- token, cookie, credential, secret, signed URL, webhook, or env values
- raw OCR, transcript, scraping, vendor, or integration payloads

Customer-facing surfaces should use reviewed labels, redacted identifiers, and
approved evidence summaries only.

## Screenshot And Evidence Policy

This gate does not create screenshots, images, videos, renders, fixtures, or
asset files.

Future screenshot or evidence-capture work should follow these rules:

- use synthetic or approved demo data unless a separate approval gate allows
  real account data
- redact advertiser, account, user, campaign, billing, token, cookie, and
  internal operational identifiers
- do not include production dashboard screenshots in docs or tests without
  explicit approval
- store only the minimum evidence needed for QA traceability
- mark generated or simulated evidence clearly
- keep screenshots and evidence artifacts out of this docs-only gate

## Production API Boundary

Do not call production APIs for this gate.

Future production API access requires a separate explicit approval gate that
states:

- endpoint or system being accessed
- read/write behavior
- account or tenant scope
- data handling and redaction plan
- evidence retention policy
- rollback or cleanup plan if any state can change

Until that approval exists, Compass evidence QA should use documentation,
synthetic examples, local fixtures, or non-production approved sources only.

## Acceptance Criteria For A Future Implementation Gate

A downstream implementation gate should verify:

- each Compass evidence item resolves to exactly one displayed state
- `noData` cannot be rendered as evidence-supported
- `generation-limited` cannot hide its limitation reason
- source panel fields are redacted before customer-facing display
- unsupported generated claims are visually and structurally separated from
  verified facts
- screenshot and evidence artifacts use approved data only
- production API access remains disabled unless separately approved

## No-Touch Confirmation

This gate does not perform:

- product code changes
- package, lockfile, or dependency changes
- image, video, render, screenshot, or asset generation
- asset copy, upload, move, delete, or modification
- production API calls
- external repo edits
- secret, env, token, cookie, or credential output

## Next Gate Suggestion

Recommended next gate:

```text
Gate Design-Director-Compass-Evidence-QA-2
```

Suggested purpose:

```text
Create a non-production implementation checklist and test fixture plan for the
Compass evidence QA states, including redaction checks and source panel field
mapping. Keep production API access, screenshots, generated media, and asset
changes out of scope unless separately approved.
```
