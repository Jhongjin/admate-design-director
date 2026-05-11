# Design Director Intelligence Library Candidate QA 4 Product Handoff Note v1

Date: 2026-05-11
Gate: Design-IntelLib-Candidate-QA-4
Status: docs-only handoff
Depends on:
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_2_wireframe_acceptance_checklist_v1.md`
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_3_fixture_state_copy_pack_v1.md`

Scope: package the Intelligence Library candidate QA guidance for a future
product implementation or product-side fixture planning gate. This gate does
not implement UI, call APIs, modify DB/auth/env, create screenshots, generate
media, or edit product repos.

## Handoff Summary

The Intelligence Library candidate review surface should treat each insight as
a governed object, not loose text.

The implementation should preserve these four principles:

```text
State before body.
Source basis before reuse.
Scope before export.
Safety before recommendation.
```

## Product Team Inputs

Future product-side planning should define:

- destination repo and route
- list/detail/review surfaces
- allowed user roles
- source metadata model
- state transition model
- owner/reviewer model
- expiry and stale-content policy
- export/copy/reuse boundaries

## Required State Coverage

The first product fixture plan should include:

- candidate
- draft
- in-review
- approved
- needs-update
- deprecated
- blocked
- empty list
- filtered no-results
- source unavailable

## Minimum Acceptance Criteria

Before visual QA, the product surface should prove:

- state is visible before insight body
- approved scope appears before reuse/export actions
- blocked items cannot be copied or exported
- source basis is visible without a deep modal
- stale or deprecated items are not recommended by default
- mobile layout has no horizontal overflow for Korean state copy
- screenshots use synthetic fixture data only

## Copy Pack To Reuse

Use the QA-3 copy pack as a starting point for:

- state badges
- detail panel source basis
- blocking reasons
- empty states
- action success/failure copy

Product teams may adjust tone, but should not remove source basis, scope,
freshness, or safety meaning.

## Risks To Avoid

P0 risks:

- candidate or draft content appears approved
- blocked or deprecated content is recommended
- private source details appear in UI, logs, prompts, exports, or screenshots
- source basis is hidden behind too many interactions
- state transition actions are ambiguous

P1 risks:

- reviewer checklist is too verbose
- mobile action hierarchy hides state
- confidence and freshness labels are copy-only without visual hierarchy
- replacement path is unclear for deprecated content

## Suggested Next Product Gate

```text
Gate Intelligence-Library-1 route and fixture inventory
```

Suggested scope:

- read-only route inventory
- existing data model inventory
- local fixture candidate list
- no DB mutation
- no production API calls
- no screenshot generation

## No-Touch Confirmation

This gate does not perform:

- product repo edits
- UI implementation
- API or DB changes
- auth, env, or storage changes
- screenshot capture
- generated media or asset creation
- production traffic
- real customer data review
- secret, env, token, cookie, session, credential, signed URL, raw provider, or
  raw source output

