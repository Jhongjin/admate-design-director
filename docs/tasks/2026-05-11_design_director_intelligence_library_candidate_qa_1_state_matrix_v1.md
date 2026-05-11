# Design Director Intelligence Library Candidate QA 1 State Matrix v1

Date: 2026-05-11
Gate: Design-IntelLib-Candidate-QA-1
Status: docs-only gate
Depends on:
- `docs/reviews/design-gate-priority-map-v1.md`
- `docs/tasks/2026-05-10_design_director_compass_evidence_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-11_design_director_foresight_benchmark_qa_1_state_matrix_v1.md`

Scope: define candidate state, review, and handoff expectations for an AdMate
Intelligence Library surface before product implementation. This gate does not
change product code, API, DB, auth, env, screenshots, generated media, assets,
or product repo files.

## Purpose

The Intelligence Library should make reusable marketing intelligence easier to
trust, reuse, and retire.

The Design Director position is:

```text
Intelligence is not a pile of notes.
It is a governed object with state, source basis, owner intent, and expiry.
```

This document defines a first QA matrix for future candidate review screens,
draft libraries, source panels, approval workflows, and stale content handling.

## Candidate State Matrix

| State | Meaning | Required UI expectation | Failure signal |
| --- | --- | --- | --- |
| `candidate` | Newly captured or proposed insight that has not been reviewed. | Show source basis, proposed use, owner, and review action. | Candidate looks reusable before review. |
| `draft` | Human or agent has shaped the candidate but it is not approved. | Mark as draft and keep approval actions visible. | Draft is shown in library search as approved guidance. |
| `in-review` | Reviewer is evaluating source, wording, safety, and applicability. | Show reviewer queue state, last review time, and blocking checklist. | User cannot tell whether review is active or abandoned. |
| `approved` | Content is approved for reuse in the defined scope. | Show approved scope, source basis, freshness, and reuse entrypoints. | Approved object lacks source or expiry context. |
| `needs-update` | Content is still useful but stale, partial, or affected by changed policy. | Show update reason and suppress confident reuse affordances. | Stale guidance appears current. |
| `deprecated` | Content should not be used for new work but may remain for history. | Keep history visible and make replacement path explicit. | Deprecated content is recommended by default. |
| `blocked` | Content cannot proceed due to unsafe source, missing basis, private data risk, or unclear ownership. | Show sanitized blocking reason and safe next action. | Sensitive or unsupported content remains searchable as normal guidance. |

## Trust Display Matrix

Every reusable intelligence object should expose these dimensions:

| Trust dimension | Display expectation |
| --- | --- |
| Source basis | Human-readable origin class, source count, and evidence status. |
| Scope | Product, channel, market, platform, or workflow where reuse is allowed. |
| Owner | Team or role responsible for maintaining the object. |
| Review status | Candidate, draft, in-review, approved, needs-update, deprecated, or blocked. |
| Freshness | Created, reviewed, and expiry or recheck timing. |
| Confidence | Clear confidence label with reason, not color-only. |
| Safety boundary | Private data, external claim, policy, and mock-data flags where relevant. |
| Replacement path | For stale or deprecated content, show successor or request-update action. |

## Candidate Review QA

Pass criteria:

- candidate state is visible before title/body detail
- source basis is visible without opening a deep inspector
- reviewer actions are explicit and mutually exclusive
- blocked state shows a sanitized reason and does not expose private source data
- long Korean labels and mixed English terms wrap safely
- approved scope is visible before reuse/export actions

P0 blockers:

- private account, campaign, billing, personal, credential-like, or raw provider
  details appear in UI or QA evidence
- candidate or draft content is presented as approved guidance
- stale or deprecated content is recommended without warning
- blocked content remains available in default reuse flows

P1 follow-ups:

- reviewer checklist is too long for repeated work
- source basis is understandable only to engineers
- stale/replacement path needs clearer hierarchy
- mobile review queue needs stacked treatment
- confidence copy needs tighter wording

## Source Panel QA

Future source/evidence panels should confirm:

- source basis and source type are visible
- source details are summarized without raw private payload
- evidence count and recency are shown together
- source confidence explains why the object can or cannot be reused
- blocked sources do not leak sensitive snippets in preview cards

Design QA should not change retrieval, indexing, ingestion, DB write, or source
redaction logic.

## Reuse And Handoff QA

Approved intelligence may be handed off to product teams only when:

- reuse scope is explicit
- owner and review date are visible
- stale and deprecated states are excluded from default recommendations
- blocked objects cannot be exported, copied into prompts, or used as templates
- destination product copy preserves confidence, safety, and source basis labels

## Mobile QA

Mobile review surfaces should prioritize:

- state and owner above source detail
- a single primary review action
- collapsible evidence/source section
- no horizontal overflow for Korean labels, platform names, or long titles
- sticky actions only when they do not cover status or evidence text

## No-Touch Boundary

This gate does not authorize:

- product repo changes
- API or DB changes
- crawler, indexing, import, retrieval, or embedding changes
- auth, env, or storage changes
- screenshot capture or generated media
- real customer data review
- production traffic or mutation

## Next Gate

Recommended next gate:

```text
Gate Design-IntelLib-Candidate-QA-2 local wireframe acceptance checklist
```

That gate should translate this state matrix into a review-screen checklist
without creating assets, screenshots, or product implementation.
