# Design Director Intelligence Library Candidate QA 2 Wireframe Acceptance Checklist v1

Date: 2026-05-11
Gate: Design-IntelLib-Candidate-QA-2
Status: docs-only gate
Depends on:
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_1_state_matrix_v1.md`

Scope: define wireframe acceptance criteria for a future Intelligence Library
candidate review screen. This gate does not create screenshots, generated
media, assets, product UI code, API changes, DB changes, auth changes, or
production traffic.

## Purpose

The first Intelligence Library review screen should let operators answer four
questions quickly:

```text
What is this insight?
Can I trust its source basis?
Where can it be reused?
What should happen next?
```

This checklist turns the state matrix into wireframe-level acceptance criteria
without prescribing final visuals.

## Required Regions

| Region | Required content | Acceptance criteria |
| --- | --- | --- |
| Header | title, state, owner, reviewed/freshness label | State is visible before body text. |
| Summary | short insight body and intended use | Body does not hide review status. |
| Source basis | source count, source type, evidence status | Source basis is visible without a deep modal. |
| Scope | product/channel/platform/market/workflow | Reuse scope appears before export or copy actions. |
| Safety | private data, claim, policy, mock-data flags | Blocking flags cannot be missed. |
| Actions | approve, request update, block, deprecate | Primary action matches current state. |
| History | created/reviewed/deprecated timestamps | Review history does not expose raw private values. |

## State-specific Wireframe Rules

### Candidate

- show "candidate" state near the title
- show review checklist before reuse action
- default primary action is review, not export
- source basis and intended use must be visible

### Draft

- show draft badge and last edited label
- allow request-update or submit-for-review action
- hide approved reuse affordances

### In-review

- show assigned reviewer or review queue label
- show blocking checklist progress
- prevent duplicate approval actions if review is already active

### Approved

- show approved scope and source basis
- allow reuse/copy/export only with confidence and safety labels attached
- keep review/expiry date visible

### Needs-update

- show update reason above reuse actions
- reuse action should be secondary or disabled depending on severity
- replacement or request-update action should be prominent

### Deprecated

- show replacement path when available
- remove from default recommendation flows
- keep historical read-only context

### Blocked

- show sanitized block reason
- disable reuse/export/copy
- show safe next action
- never expose raw source payload or private identifiers

## Mobile Acceptance

Mobile wireframes pass only if:

- state, title, and primary action fit in the first screen
- source basis collapses but remains reachable
- action buttons do not cover evidence text
- long Korean titles and mixed English platform names wrap without horizontal
  scrolling
- blocked and needs-update warnings remain visible before reuse actions

## Empty And Loading States

Required states:

- no candidates
- filter returns no candidates
- source basis loading
- review action pending
- review action failed
- stale content detected

No empty state may imply that missing evidence is equivalent to approval.

## Copy Guardrails

Use:

- "검토 전 후보입니다."
- "승인된 범위에서만 재사용할 수 있습니다."
- "근거가 부족해 업데이트가 필요합니다."
- "차단된 항목은 복사하거나 내보낼 수 없습니다."

Avoid:

- "안전함" without source basis
- "검증 완료" for draft or candidate states
- "추천" for deprecated or blocked content
- raw implementation terms, table names, or source pipeline labels

## No-Touch Confirmation

This gate does not perform:

- product repo edits
- API or DB changes
- auth, env, or storage changes
- screenshot capture
- generated media or asset creation
- production traffic
- real customer data review
- secret, env, token, cookie, session, credential, signed URL, raw provider, or
  raw source output

## Next Gate

Recommended next gate:

```text
Gate Design-IntelLib-Candidate-QA-3 fixture-state copy pack
```

That gate can define localized copy for candidate, draft, approved,
needs-update, deprecated, and blocked states without product implementation.
