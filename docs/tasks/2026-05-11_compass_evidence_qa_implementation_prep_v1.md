# Design Director Compass Evidence QA Implementation Prep v1

Date: 2026-05-11
Gate: Compass-Evidence-QA-Implementation-Prep
Status: docs-only planning gate
Depends on:
- `docs/tasks/2026-05-10_design_director_compass_evidence_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-10_design_director_compass_evidence_qa_2_fixture_test_plan_v1.md`
- `docs/tasks/2026-05-11_design_director_compass_evidence_qa_3_fixture_review_prep_v1.md`

Scope: prepare the Compass evidence QA implementation packet that a future
product Agent must complete before any product repo work begins. This document
does not create fixtures, edit product code, approve final destination paths,
call APIs, touch RAG/DB/auth/env, capture screenshots, or generate media.

## Purpose

Compass evidence QA needs a controlled handoff from Design Director planning to
product-side implementation.

This prep package exists to keep the future work narrow:

```text
First approve the fixture plan.
Then approve the destination.
Only then implement synthetic local tests.
```

## Implementation Prep Checklist

The future product Agent should provide the following before implementation:

| Input | Required detail | Approval needed before |
| --- | --- | --- |
| Destination repo | Product repo name, branch, route, and owning surface. | Any file read beyond inventory. |
| Final destination paths | Proposed fixture, test, helper, and component paths. | Creating or editing files. |
| Fixture list | One expected evidence state per fixture. | Writing fixture records. |
| Source-panel mapping | Safe display labels for title, type, freshness, review state, scope, and limitation reason. | Rendering or snapshot work. |
| Redaction checks | Guarded values, raw payloads, internal identifiers, and source internals to exclude. | Test implementation. |
| Commands | Exact local commands and whether they read, build, or test only. | Execution in product repo. |
| No-touch confirmation | RAG, API, DB, crawler, reembedding, auth, env, production traffic, screenshots, and media excluded. | Any implementation. |

## Required State Inventory

The proposed implementation should cover:

- `source-found`
- `noData`
- `generation-limited`
- rejected or stale source
- partial evidence
- verified source preserved during answer generation failure
- long Korean and mixed English display stability
- customer-safe source panel mapping
- internal field redaction
- empty source result that cannot look supported

Each fixture should state the expected UI label, source-panel display fields,
and blocked fields.

## Safe Fixture Shape

Future fixture records may use the product repo's established test format.
Conceptually, each record should define:

- request label
- expected evidence state
- answer summary
- source label
- source category
- freshness or reviewed date label
- review status
- limitation or no-data reason
- checked scope
- expected displayed text
- expected excluded text

These labels are planning concepts only. They do not require API contract,
schema, RAG, or DB changes.

## Redaction Prep

Future implementation must verify that rendered text and test output exclude:

- raw source payloads
- private dashboard paths
- provider raw responses
- account, tenant, workspace, advertiser, campaign, creative, audience, trace,
  run, job, vector, embedding, retrieval, or reviewer identifiers
- prompt internals and model traces
- credential, cookie, signed URL, env, webhook, and key-like values
- internal field names exposed as customer-facing copy

Synthetic placeholders are allowed only when clearly marked as mock data and
not shaped like real production identifiers.

## Route And Destination Approval

This document does not approve the final implementation destination.

The future product Agent must ask for a separate destination approval with:

- exact route name
- exact fixture path
- exact test path
- exact helper path if needed
- expected changed file list
- reason each file belongs in scope
- confirmation that unrelated files will not be touched

No product repo file should be created or modified until that approval is
granted.

## Future Validation Plan

Once implementation and destination are approved, validation should stay local
and synthetic:

- run existing unit/component tests that cover the selected route
- run targeted fixture state tests
- run text redaction assertions
- run build or typecheck only if already standard for the product repo
- run whitespace diff checks
- avoid screenshots, browser capture artifacts, media generation, production
  API calls, DB reads/writes, RAG import, crawler work, and reembedding

## Blockers

Stop the future product gate if:

- fixture data includes real advertiser, account, campaign, billing, customer,
  private source, credential, or provider payload data
- passing the fixture requires RAG/API/DB/schema/crawler/auth/env changes
- the destination path is unclear or unapproved
- no-data and generation-limited states collapse into one UI state
- source evidence disappears when answer generation is limited but retrieval
  succeeded
- internal field names or raw source payloads are intentionally displayed
- screenshots or generated media are required for acceptance

## Product Agent Prompt

Use this prompt for the future Compass product Agent:

```text
You are the AdMate Compass product Agent.

Gate:
Compass-Evidence-QA-Implementation-Approval

Goal:
Submit an implementation proposal for synthetic local Compass evidence QA
fixtures and tests.

Do not implement yet.
Do not create fixtures yet.
Do not approve or write final destination paths yourself.
Do not call production APIs.
Do not modify RAGSearchService, chat API routes, DB/schema/import,
reembedding, crawler, auth, env, dependencies, or secrets.
Do not capture screenshots or generate image/video/audio/render assets.
Do not expose real advertiser, account, campaign, private source, credential,
cookie, signed URL, provider payload, or raw source data.

Required output:
- destination repo and route
- proposed fixture and test paths
- fixture list with expected evidence state
- source-panel display mapping
- redaction assertions
- local commands proposed for validation
- explicit implementation and final destination approval request
- no-touch confirmation
```

## No-Touch Confirmation

This gate does not perform:

- product repo edits
- fixture creation
- test implementation
- package, lockfile, or dependency changes
- API, RAG, DB, auth, env, crawler, reembedding, or storage changes
- screenshot capture
- image, video, audio, render, media, or asset generation
- production traffic
- secret, credential, cookie, signed URL, raw provider, raw source, or real
  customer data output

## Future Gates

Required future gates:

```text
Gate Compass-Evidence-QA-Implementation-Approval
Gate Compass-Evidence-QA-Final-Destination-Approval
```

Implementation and final destination approval are both future gates. This
document is only a planning packet.
