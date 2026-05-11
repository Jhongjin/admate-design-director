# Design Director Compass Evidence QA 3 Fixture Review Prep v1

Date: 2026-05-11
Gate: Design-Director-Compass-Evidence-QA-3
Status: docs-only gate
Depends on:
- `docs/tasks/2026-05-10_design_director_compass_evidence_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-10_design_director_compass_evidence_qa_2_fixture_test_plan_v1.md`

Scope: prepare a Design Director review checklist for proposed Compass
evidence fixtures and source-panel tests. This gate does not create fixtures,
change product code, call production APIs, run browser QA, add screenshots, or
edit external repos.

## Purpose

The next Compass evidence QA implementation work should be reviewed before any
fixture, test, UI, API, RAG, or database change lands in the product repo.

This document gives Design Director a compact review frame for judging whether
future Compass fixtures prove the right user-facing states:

- `source-found`
- `noData`
- `generation-limited`
- long Korean and mixed-language display stability
- internal field redaction
- verified source preservation during answer generation failure

The review focus is trust signaling. Compass should not make an answer feel
more supported than the available source evidence allows.

## Review Inputs

A future product Agent should submit only non-production fixture proposals or
test-case outlines unless a separate approval gate allows broader execution.

Required review inputs:

- fixture list with expected state for each case
- proposed source/evidence panel display fields
- redaction expectations
- no-data copy
- generation-limited copy
- long Korean answer/source example
- desktop and mobile layout test plan
- commands that will be run in the product repo
- explicit no-touch confirmation for RAG, API, DB, crawler, reembedding,
  auth, env, and production traffic

Do not accept a fixture review packet that includes real advertiser names,
campaign names, account identifiers, private URLs, production dashboard
screenshots, tokens, cookies, signed URLs, raw source payloads, or raw provider
responses.

## Fixture Review Matrix

| Fixture | Required expected state | Must prove | Failure signal |
| --- | --- | --- | --- |
| Verified Meta policy answer | `source-found` | Source title, source category, and freshness/review status remain visible near the answer. | Answer appears supported only by a count, confidence color, or generated prose. |
| Generic valid policy question | `source-found` | Generic policy guidance can preserve verified sources without overclaiming platform specificity. | Source panel looks decorative or unrelated to the answer. |
| Future/impossible policy question | `noData` | Compass can decline evidence support without fabricating a policy basis. | Generic sources are attached as if they validate the impossible request. |
| Fictional platform question | `noData` | Unsupported platform-like terms do not inherit real-platform evidence. | Real platform sources are shown as support for the fictional platform. |
| Fictional product on real platform | `source-found` or limited state as product repo policy defines | Real platform policy evidence can remain visible without claiming product-specific validation. | The UI implies the fictional product itself was verified. |
| Generation failure with sources | `generation-limited` | Verified sources remain visible when answer generation is limited or fails. | Source panel disappears or collapses into a generic error. |
| Rejected or expired source | `noData` or `generation-limited` | Rejected, stale, or expired evidence cannot silently upgrade a claim to supported. | Rejected/stale source appears as accepted support. |
| Long Korean policy answer | Any state | Korean, English platform names, numbers, and URLs wrap without overlap on desktop and mobile. | Badge, source title, URL, answer text, or input controls overflow. |

## Source Panel Field Mapping Review

The product fixture should map source internals into reviewed display fields.

Required display concepts:

- source title or reviewed evidence label
- source category or source type
- freshness, capture date, observed date, or refresh label when available
- review state such as accepted, limited, expired, rejected, unavailable, or
  unreviewed
- concise no-data or limitation reason when applicable
- checked scope when applicable

Internal implementation fields may exist in fixture data, but customer-facing
UI should not expose raw field names such as:

- `retrievalMethod`
- `sourceQuality`
- `hybridScore`
- `sourcesCount`
- `ollama_document_chunks`
- `schema=compass`
- raw trace, job, run, vector, embedding, or retrieval identifiers

If a future implementation needs source counts, the display copy should use a
reviewed label such as "확인된 출처" rather than raw API field names.

## State Copy Review

### `source-found`

Copy should make the answer feel evidence-supported only to the degree the
source panel can prove.

Pass examples:

- "확인된 출처를 바탕으로 요약했습니다."
- "근거 문서를 함께 확인해 주세요."

Fail examples:

- "정책상 안전합니다."
- "Compass가 검증했습니다."
- "출처 3개" with no inspectable source labels.

### `noData`

Copy should be vendor-neutral and should not imply a policy conclusion.

Pass examples:

- "요청한 범위에서 확인 가능한 근거를 찾지 못했습니다."
- "플랫폼명이나 정책 범위를 좁혀 다시 확인해 주세요."

Fail examples:

- "허용됩니다."
- "문제가 없습니다."
- "Google/Meta/Naver 쪽 문제입니다."

### `generation-limited`

Copy should separate answer-generation limits from source retrieval results.

Pass examples:

- "답변 생성은 제한되었지만 확인된 근거는 아래에서 볼 수 있습니다."
- "일부 근거만 확인되어 제한된 요약으로 표시합니다."

Fail examples:

- source retrieval success collapses into a generic failure
- no-data and generation failure use the same copy
- limitation reason is hidden behind hover-only UI

## Mobile And Long Text Review

Fixture review should require mobile checks before implementation is accepted.

Required mobile expectations:

- answer/input remains within viewport width
- source/evidence panel does not push primary answer/input off-screen
- source titles and URLs wrap or truncate predictably
- Korean policy text, English platform names, numbers, and URL-like text do not
  overlap badges or buttons
- drawers, accordions, or panels preserve the relationship between answer and
  source evidence

Desktop expectations:

- answer and source/evidence are visually connected
- source panel is not a decorative footer
- source details are scannable without making the answer look unsupported
- generated interpretation is structurally separated from verified facts

## Redaction Review

Future fixtures and tests must assert that rendered text and evidence packets
do not include:

- raw source payloads
- account, tenant, workspace, advertiser, campaign, creative, audience, trace,
  job, run, vector, embedding, or retrieval IDs
- reviewer identity or internal comments
- raw OCR, transcript, scraping, vendor, or integration payloads
- token, cookie, credential, secret, signed URL, webhook, or env values
- private dashboard paths or raw provider responses

Synthetic placeholders are acceptable only when they are clearly labeled as
mock data and do not resemble real production identifiers.

## Acceptance Criteria For The Future Product Gate

A future Compass implementation gate should be allowed to proceed only if the
fixture proposal satisfies all of the following:

- every fixture has one expected evidence state
- `noData` cannot be rendered as evidence-supported
- `generation-limited` preserves verified sources when retrieval succeeded
- source panel fields map to customer-safe labels
- internal field names are not shown in user-facing UI
- rejected, expired, partial, or missing evidence cannot silently upgrade to
  supported
- mobile long text and source panel behavior are explicitly tested
- fixture data is synthetic, local, non-production, or otherwise approved
- commands are limited to product repo test/build/harness checks
- production API access, screenshots, and asset creation are excluded unless
  separately approved

## Blockers

Stop the future product gate if any of these are true:

- fixture proposal needs production API access without explicit approval
- fixture proposal includes real advertiser, account, campaign, billing, or
  private source data
- source panel requires API/RAG/DB behavior changes to pass
- test implementation would modify crawler, ingestion, reembedding, ranking,
  answer generation, auth, env, or schema
- screenshots or evidence artifacts would capture production dashboard data
- raw internal field names are intentionally exposed to customers

## Product Agent Prompt

Use this prompt when handing the next implementation-planning review to the
Compass product Agent:

```text
You are the AdMate Compass product Agent.

Gate:
Compass-Evidence-QA-Implementation-Prep

Goal:
Submit a non-production fixture and test proposal for Compass evidence states
before implementation.

Read first:
- Design Director Compass Evidence QA state matrix
- Design Director Compass Evidence QA fixture test plan
- current Compass /chat-ollama UI and source/evidence display files
- current local test and harness conventions

Do not modify files in this Gate.
Do not call production APIs.
Do not create screenshots or assets.
Do not modify RAGSearchService, /api/chat-ollama, DB/schema/import,
reembedding, crawler, auth, env, package dependencies, or secrets.

Required output:
- proposed fixture list
- expected state for each fixture
- source panel field mapping
- noData copy
- generation-limited copy
- redaction checks
- desktop/mobile layout checks
- commands proposed for the implementation Gate
- no-touch confirmation

Keep all fixture data synthetic or approved non-production.
Do not include token, cookie, credential, secret, signed URL, raw source payload,
raw provider response, or private customer data.
```

## No-Touch Confirmation

This gate does not perform:

- product code changes
- fixture creation
- test implementation
- package, lockfile, or dependency changes
- production API calls
- local or production database reads/writes
- screenshot, image, video, render, generated media, or asset creation
- external repo edits
- secret, env, token, cookie, credential, signed URL, or private URL output

## Next Gate

Recommended next gate:

```text
Gate Compass-Evidence-QA-Implementation-Prep
```

Suggested purpose:

```text
Have the Compass product Agent submit the proposed synthetic fixture/test plan
for Design Director review. Keep implementation, screenshots, production API
access, and RAG/API/DB changes out of scope until that proposal is accepted.
```
