# Design Director Compass Evidence QA 2 Fixture Test Plan v1

Date: 2026-05-10
Gate: Design-Director-Compass-Evidence-QA-2
Status: docs-only gate
Depends on: `docs/tasks/2026-05-10_design_director_compass_evidence_qa_1_state_matrix_v1.md`
Scope: define a non-production fixture and test plan for Compass evidence QA. This gate does not create fixtures, change product code, call production APIs, add screenshots/assets, or edit external repos.

## Purpose

This gate converts the Compass evidence QA state matrix into a fixture and
test-plan outline that a future implementation gate can use in a
non-production environment.

The plan covers:

- fixture categories for `source-found`, `noData`, and `generation-limited`
- redaction checks for customer-facing source and evidence display
- source panel field mapping
- expected behavior for no-data and generation-limited states
- acceptance criteria for a later implementation gate
- no-touch boundaries for this docs-only gate

## Non-Production Fixture Categories

Future fixtures should be synthetic, local, or otherwise approved
non-production records. They should be small enough to inspect manually and
explicit enough to prove state behavior without relying on production systems.

| Fixture category | Purpose | Minimum records | Expected state coverage |
| --- | --- | --- | --- |
| Verified source fixture | Proves that a Compass output can attach to an acceptable evidence record. | One claim or answer, one source record, one source panel payload. | `source-found` |
| Missing evidence fixture | Proves that Compass does not fabricate support when no usable evidence exists. | One request scope, one empty evidence result, one explicit no-data reason. | `noData` |
| Limited generation fixture | Proves that generated output remains visibly limited when evidence is incomplete, stale, redacted, low-confidence, or policy-limited. | One answer summary, one limitation reason, optional partial source summary. | `generation-limited` |
| Redacted identifier fixture | Proves internal and sensitive identifiers are transformed before display. | One source record containing synthetic internal IDs and one expected redacted display payload. | `source-found`, `generation-limited` |
| Stale or expired source fixture | Proves stale evidence cannot be silently presented as fully supported. | One dated source record with an expired or limited review status. | `generation-limited` |
| Rejected source fixture | Proves rejected evidence does not upgrade a claim to supported. | One source record marked rejected and one claim that would otherwise cite it. | `noData` or `generation-limited` |
| Partial source fixture | Proves partial source summaries stay separate from verified facts. | One partial source summary and one generated interpretation. | `generation-limited` |
| Long mixed-language fixture | Proves Korean, English, platform names, numbers, and URLs remain stable in source panel fields. | One long answer and one long source excerpt. | Any state with display fields |

Fixture records should be labeled as synthetic and should not contain real
advertiser names, account names, campaign names, user names, customer URLs,
billing data, credentials, tokens, cookies, signed URLs, or private dashboard
content.

## Fixture Data Shape

A future non-production fixture may use any local format that matches the
product repo's test conventions. The record should still expose enough fields
to test the source panel behavior from the state matrix.

Recommended conceptual fields:

- `evidenceState`
- `claimLabel`
- `answerSummary`
- `sourceTitle`
- `sourceCategory`
- `sourceType`
- `capturedAt`, `observedAt`, or `refreshedAt`
- `reviewStatus`
- `evidenceRecordId`
- `limitationReason`
- `noDataReason`
- `checkedScope`
- `partialSourceSummary`
- `generatedInterpretation`
- `redactionInput`
- `expectedDisplay`

These names are fixture-planning labels only. A future implementation gate
should map them to the product repo's real local test helpers and should not
change production API contracts just to match this document.

## Source Panel Field Mapping

The source panel should map internal or fixture fields into reviewed,
customer-safe display fields.

| Review question | Fixture/source input | Customer-facing display expectation |
| --- | --- | --- |
| What evidence supports this output? | `sourceTitle`, `evidenceRecordId`, `claimLabel` | Source title or reviewed evidence label. Internal record IDs are hidden or redacted unless explicitly approved for display. |
| Where did it come from? | `sourceCategory`, `sourceType` | Reviewed category such as first-party account data, uploaded reference, approved benchmark, policy note, or analyst-authored note. |
| When was it captured or refreshed? | `capturedAt`, `observedAt`, `refreshedAt` | Human-readable date or freshness label when available. Missing dates should not imply current evidence. |
| What is the review state? | `reviewStatus`, `evidenceState` | User-facing status such as accepted, limited, expired, rejected, unavailable, or unreviewed. |
| Why is support missing or limited? | `noDataReason`, `limitationReason`, `checkedScope` | Concise reason and checked scope where available. Avoid blame, speculation, or policy conclusions without evidence. |
| What part is generated interpretation? | `generatedInterpretation`, `partialSourceSummary` | Generated interpretation is visually and structurally separate from verified source facts. |

Internal fixture labels such as `evidenceState`, `reviewStatus`, or
`evidenceRecordId` may appear in test data, but customer-facing UI should use
reviewed labels and redacted identifiers.

## Redaction Checks

Future tests should assert that customer-facing source panels, exports,
screenshots, review packets, and demo material do not expose guarded fields
from the state matrix.

Required checks:

- raw source payloads are not displayed by default
- account, tenant, workspace, advertiser, campaign, creative, and audience IDs
  are removed or replaced with approved labels
- prompt, model, trace, job, run, vector, embedding, retrieval, and confidence
  internals are not exposed as raw field names
- reviewer identity, internal comments, policy labels, and escalation notes are
  hidden unless explicitly approved
- tokens, cookies, credentials, secrets, signed URLs, webhook values, and env
  values never appear in fixture output or test snapshots
- raw OCR, transcript, scraping, vendor, and integration payloads are summarized
  or excluded
- long URLs are displayed as approved hostnames or safe labels when the full
  URL would reveal private paths or query parameters

Suggested negative assertions:

- no rendered text contains `token`, `cookie`, `secret`, `authorization`,
  `bearer`, `signedUrl`, `apiKey`, `password`, or `privateKey`
- no rendered text contains raw internal field names such as `retrievalMethod`,
  `sourceQuality`, `schema=compass`, or raw `sourcesCount`
- no rendered text contains synthetic internal identifiers except the approved
  redacted placeholders used by the fixture

## State Expectations

### `source-found`

Expected behavior:

- the evidence item resolves to `source-found`
- source title or label is visible
- source category or type is visible when available
- capture, observation, or refresh date is visible when available
- review status is visible when available
- verified source facts are distinguishable from generated interpretation
- missing optional fields do not create false confidence

Failure examples:

- source count appears without a source title or inspectable label
- generated interpretation is presented as the source of record
- raw evidence payload or internal identifiers are displayed
- rejected, expired, or missing evidence is shown as supported

### `noData`

Expected behavior:

- the evidence item resolves to `noData`
- the UI does not show the output as evidence-supported
- a concise no-data reason appears when available
- checked scope appears when available
- copy stays vendor-neutral and does not speculate about policy permission
- empty source shells do not look like validated evidence

Failure examples:

- no-data state says or implies the recommendation is safe
- a source panel shell appears with empty fields but supported styling
- vendor or platform blame appears without evidence
- generation failure is collapsed into no-data without explanation

### `generation-limited`

Expected behavior:

- the evidence item resolves to `generation-limited`
- the limitation reason is visible before the user relies on the answer
- partial source summaries stay separate from verified source facts
- stale, redacted, rejected, incomplete, or low-confidence evidence does not
  upgrade the answer to supported
- verified sources remain visible when retrieval succeeded but answer
  generation was limited or failed

Failure examples:

- limitation reason is hidden behind a hover-only or collapsed-only treatment
- generated answer appears fully supported even when evidence is partial
- available verified sources disappear after generation failure
- no-data and generation-limited are represented as the same state

## Test Plan For Future Implementation Gate

A future implementation gate should run fixture-backed tests in the product
repo after reading the product repo's local test conventions.

Recommended test groups:

- state resolution tests for `source-found`, `noData`, and
  `generation-limited`
- source panel mapping tests for title, category, date, review state, checked
  scope, and limitation reason
- redaction tests for guarded identifiers, secrets, raw payloads, and internal
  field names
- separation tests for verified facts vs generated interpretation
- no-data copy tests for vendor-neutral, non-speculative behavior
- generation-limited tests for limitation visibility and source preservation
- long mixed-language display tests for Korean, English, numbers, URLs, and
  compact badge text
- snapshot or DOM text tests using only synthetic non-production fixture data

Out-of-scope for this docs-only gate:

- creating the actual fixture files
- writing product tests
- changing UI, API, retrieval, database, auth, environment, or model behavior
- capturing screenshots or visual assets
- calling production APIs

## Acceptance Criteria

A future implementation gate passes only if:

- every fixture resolves to exactly one displayed evidence state
- `noData` cannot be rendered as evidence-supported
- `generation-limited` always exposes its limitation reason
- source panel display fields are mapped from safe reviewed values
- customer-facing output hides raw source payloads and guarded internals
- generated interpretation is separated from verified source facts
- rejected, expired, missing, or partial evidence cannot silently upgrade to
  supported
- verified sources remain visible when answer generation is limited but source
  retrieval succeeded
- fixture data is synthetic, approved non-production, or otherwise explicitly
  cleared for test use
- tests avoid screenshots, production API calls, generated media, and asset
  changes unless a later gate explicitly approves them

## No-Touch Boundary

This gate does not perform:

- product code changes
- test or fixture file creation outside this docs task file
- package, lockfile, or dependency changes
- production API calls
- local or production database reads/writes
- screenshot, image, video, render, generated media, or asset creation
- asset copy, upload, move, delete, or modification
- external repo edits
- secret, env, token, cookie, credential, signed URL, or private URL output

This document is a planning artifact only. It should not be treated as approval
to access production systems or expose real customer data.

## Next Gate

Recommended next gate:

```text
Gate Design-Director-Compass-Evidence-QA-3
```

Suggested purpose:

```text
Review the product repo's proposed non-production Compass evidence fixtures and
test cases against this plan before implementation. Confirm synthetic data,
redaction coverage, source panel field mapping, noData behavior, and
generation-limited source preservation. Keep production API access,
screenshots/assets, generated media, and product behavior changes out of scope
unless separately approved.
```
