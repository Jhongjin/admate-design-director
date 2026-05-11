# Design Director Intelligence Library 1 Route Fixture Inventory v1

Date: 2026-05-11
Gate: Intelligence-Library-1-Route-Fixture-Inventory
Status: docs-only planning gate
Depends on:
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_2_wireframe_acceptance_checklist_v1.md`
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_3_fixture_state_copy_pack_v1.md`
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_4_product_handoff_note_v1.md`

Scope: define a route and fixture inventory checklist for a future
Intelligence Library product-side planning gate. This document does not inspect
or modify product repos, approve final destination paths, create fixtures, call
APIs, touch DB/RAG/auth/env, capture screenshots, or generate media.

## Purpose

The Intelligence Library queue needs a first inventory pass before any route,
fixture, or UI implementation work starts.

Design Director position:

```text
Inventory before destination.
Destination before implementation.
Implementation before visual QA.
```

This document defines what the future product Agent should inventory and submit
for approval.

## Route Inventory Checklist

The future product Agent should identify existing or proposed surfaces for:

| Surface | Inventory question | Required output |
| --- | --- | --- |
| List route | Where are insight candidates listed? | Route path, component path, current status, owner if known. |
| Detail route | Where is one insight reviewed? | Route path, state display area, source basis area, action area. |
| Review queue | Where are candidate, draft, and in-review items triaged? | Route path or missing-route note. |
| Approved library | Where are approved reusable insights shown? | Route path and reuse/export boundary. |
| Blocked/deprecated view | Where are blocked and deprecated items visible? | Route path or planned state treatment. |
| Empty states | Where are no-candidate and filtered no-result states rendered? | Component path or missing-state note. |
| Source basis panel | Where are evidence, freshness, and scope shown? | Component path and display fields. |
| Action controls | Where are approve, request update, block, deprecate, copy, or reuse actions located? | Component path and disabled-state rules. |

This inventory is read-only. It is not approval to edit any route.

## Fixture Inventory Checklist

The future product Agent should propose local synthetic fixtures for:

- `candidate`
- `draft`
- `in-review`
- `approved`
- `needs-update`
- `deprecated`
- `blocked`
- no candidates
- filtered no results
- source unavailable
- source basis present
- source basis insufficient
- long Korean title/body/scope text
- disabled copy or reuse action for unsafe states

Each fixture proposal should include:

- fixture name
- expected state badge
- expected primary action
- disabled actions
- source basis label
- scope label
- freshness label
- blocked or update reason when relevant
- expected excluded text

## Safe Data Rules

Fixtures should use synthetic labels only.

Allowed examples:

- mock insight titles
- generic product labels
- generic channel labels
- generic source-basis labels
- synthetic dates
- synthetic reviewer roles

Disallowed examples:

- real customer or advertiser names
- account, campaign, ad, tenant, billing, or workspace identifiers
- private source URLs
- raw source payloads
- exact reviewer identities
- credential, cookie, signed URL, env, provider response, or secret-like values
- production screenshots or exported artifacts

## State And Action Expectations

The inventory should prove these interaction boundaries:

- candidate and draft items are not presented as approved
- blocked and deprecated items cannot be copied, exported, or recommended by
  default
- approved items still show scope and source basis before reuse
- needs-update items show why a refresh is required
- source-unavailable state does not imply the insight is safe
- long Korean copy wraps without hiding the badge or action controls
- empty and filtered states provide safe next actions without creating content

## Final Destination Approval

This document does not approve route, fixture, test, or helper destinations.

The future product Agent must submit a separate final destination approval
packet containing:

- destination repo
- exact route paths
- exact fixture paths
- exact test paths
- exact helper paths if any
- expected changed file list
- reason each file is in scope
- validation commands
- no-touch confirmation

No product repo file should be created or modified before that approval is
accepted.

## Future Validation Plan

After implementation and destination approval, validation should remain local
and synthetic:

- route or component tests for the selected states
- disabled-action assertions for blocked and deprecated states
- redaction assertions for source and identifier fields
- long-text layout checks through DOM or component-level tests
- build or typecheck only if standard for the product repo
- no screenshots, generated media, production traffic, DB/RAG/API calls, or
  imports/uploads

## Blockers

Stop the future gate if:

- route ownership or destination is unclear
- implementation requires DB/schema/API/RAG/auth/env changes
- fixture data uses real customer, campaign, account, provider, or private
  source details
- blocked or deprecated content can be copied or exported
- candidate or draft content appears approved
- source basis is hidden behind an unapproved deep interaction
- screenshots, media generation, or production service access are required

## Product Agent Prompt

Use this prompt for the future Intelligence Library product Agent:

```text
You are the AdMate Intelligence Library product Agent.

Gate:
Intelligence-Library-1-Route-Fixture-Inventory-Approval

Goal:
Submit a read-only route inventory and synthetic fixture proposal for
Intelligence Library candidate, review, approved, update, deprecated, blocked,
and empty states.

Do not implement yet.
Do not create fixtures yet.
Do not approve or write final destination paths yourself.
Do not call production APIs.
Do not read or write DB/RAG data.
Do not change auth, env, schema, storage, import, export, or recommendation
logic.
Do not capture screenshots or generate image/video/audio/render assets.
Do not expose real customer, advertiser, account, campaign, private source,
credential, cookie, signed URL, provider payload, or raw source data.

Required output:
- route inventory
- proposed fixture inventory
- expected state and action mapping
- proposed file destinations
- validation commands
- explicit implementation and final destination approval request
- no-touch confirmation
```

## No-Touch Confirmation

This gate does not perform:

- product repo inspection or edits
- route creation or modification
- fixture creation
- test implementation
- API, DB, RAG, auth, env, storage, import, export, or recommendation changes
- screenshot capture
- image, video, audio, render, media, or asset generation
- production traffic
- secret, credential, cookie, signed URL, raw provider, raw source, or real
  customer data output

## Future Gates

Required future gates:

```text
Gate Intelligence-Library-1-Route-Fixture-Inventory-Approval
Gate Intelligence-Library-1-Final-Destination-Approval
```

Implementation and final destination approval remain future gates. This file is
only a docs-only planning inventory.
