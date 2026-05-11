# Design Director Foresight Benchmark QA 2 Local UI State Fixture Plan v1

Date: 2026-05-11
Gate: Foresight-Benchmark-QA-2-Local-UI-State-Fixture-Plan
Status: docs-only planning gate
Depends on:
- `docs/tasks/2026-05-11_design_director_foresight_benchmark_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-11_design_director_foresight_benchmark_qa_2_data_trust_handoff_checklist_v1.md`

Scope: define a safe local UI state fixture plan for a future Foresight
benchmark QA implementation gate. This document does not create fixtures,
modify the Foresight product repo, approve a final destination path, run data
imports, call APIs, touch DB/RAG/auth/env, capture screenshots, or generate
media assets.

## Purpose

The next Foresight queue should prove benchmark trust states with local,
synthetic UI fixtures before any product implementation or benchmark import is
considered.

Planning position:

```text
Fixture states may be planned here.
Fixture files, product routes, and final destination paths require a future
implementation approval gate.
```

## Planned State Coverage

| Fixture state | UI surface to prove | Minimum safe fixture concept | Must not include |
| --- | --- | --- | --- |
| `benchmark-ready` | KPI card, benchmark table, report metadata | Synthetic aggregate metric with platform, objective, period, sample label, and confidence label. | Raw campaign rows, advertiser names, account identifiers, imports, production data. |
| `low-confidence` | KPI card, forecast panel, export preview | Synthetic metric with small sample or coverage warning and visible reason copy. | Real volume, spend, conversion, revenue, or private benchmark values. |
| `long-term-trend-only` | Trend table, chart label, report preview | Synthetic older-period benchmark separated from current benchmark. | Mixed recent and stale data without state label. |
| `validation-error` | Upload mapping panel, row/field error summary | Synthetic missing-column or invalid-mapping example with safe field names. | Real uploaded files, row payloads, parser logs, customer records. |
| `security-review-required` | Blocked promotion panel | Synthetic placeholder showing credential-like value was redacted and blocked. | Any real credential, private URL, signed link, token value, cookie, env value. |
| `raw-identifier-risk` | Report preview, export warning | Synthetic internal identifier placeholder that is excluded from report-ready output. | Account, campaign, ad set, ad, tenant, billing, or provider identifiers. |
| `no-benchmark-data` | Empty benchmark table, no-data panel | Synthetic selected scope with no usable aggregate data and a safe next action. | Fabricated forecasts, empty source shells that look supported. |

## Local Fixture Boundary

Allowed in a later implementation gate only after approval:

- local fixture records
- test helper data
- route-specific mock state adapters
- DOM or component tests using synthetic data
- read-only local dev inspection

Still blocked unless separately approved:

- benchmark import or upload
- DB reads or writes
- RAG/API calls
- production services
- provider API calls
- screenshots or media artifacts
- product repo implementation
- final destination path selection

This planning document is not approval to create fixture files in a product
repo. The future product Agent must submit the exact destination repo, route,
file paths, commands, and test scope for review before implementation.

## Display Requirements

Every local fixture should make the benchmark basis visible in the same view as
the metric.

Required display concepts:

- platform
- industry or objective
- selected metric
- date range or data window
- recent-data policy
- sample or coverage label
- confidence range or reliability label
- exclusions or rejection summary
- mock, aggregate, draft, or reviewed status

The UI must not rely on color alone for confidence, warning, blocked, stale, or
no-data states.

## Copy Requirements

Use cautious, basis-first copy:

- "Benchmark basis is limited for this scope."
- "Long-term trend reference only."
- "No usable aggregate benchmark exists for this selection."
- "Raw identifiers were excluded from report-ready output."
- "Security review is required before promotion."

Avoid overclaiming:

- "Guaranteed performance"
- "Accurate forecast"
- "Safe to export" before basis review
- "No data" without selected scope or next action

## Future Implementation Gate Inputs

Before any product repo work, the Foresight product Agent should submit:

- destination repo and route
- exact fixture file paths
- exact test file paths
- state list and expected rendered labels
- local commands to run
- data-shape mapping from fixture fields to UI labels
- redaction expectations
- confirmation that all records are synthetic or approved non-production
- confirmation that screenshots, media generation, imports, uploads, API
  calls, DB/RAG changes, and production traffic remain out of scope

## Acceptance Criteria For Future Approval

A future implementation gate may proceed only if:

- each fixture maps to one primary benchmark trust state
- fixture data is synthetic or explicitly approved non-production
- benchmark basis is visible before report/export action
- low-confidence and stale states are text-labeled
- raw identifiers and guarded values are blocked or redacted before display
- no-data state cannot fabricate a forecast
- implementation paths and final destination are approved in that future gate
- product code, tests, and fixtures are not changed by this docs-only gate

## Product Agent Prompt

Use this prompt for the future Foresight product Agent:

```text
You are the AdMate Foresight product Agent.

Gate:
Foresight-Benchmark-QA-2-Implementation-Approval

Goal:
Submit a local synthetic UI state fixture implementation proposal for
Foresight benchmark trust states.

Do not implement yet.
Do not create fixture files yet.
Do not choose or write to a final destination path without approval.
Do not run benchmark import or upload.
Do not call production APIs, provider APIs, DB, RAG, external LLMs, or retrain
jobs.
Do not capture screenshots or generate image/video/audio/render assets.
Do not expose real advertiser, account, campaign, billing, credential, env,
cookie, signed URL, raw provider, or raw row data.

Required output:
- proposed route and destination files
- local fixture list
- expected UI state for each fixture
- data-trust display mapping
- redaction checks
- local commands proposed for implementation validation
- explicit no-touch confirmation
```

## No-Touch Confirmation

This gate does not perform:

- product repo edits
- fixture creation
- test implementation
- benchmark import or upload
- API, DB, RAG, auth, env, or storage changes
- screenshot capture
- image, video, audio, render, media, or asset generation
- production traffic
- secret, credential, cookie, signed URL, raw provider, raw row, or real
  customer data output

## Future Gates

Required future gates:

```text
Gate Foresight-Benchmark-QA-2-Implementation-Approval
Gate Foresight-Benchmark-QA-2-Final-Destination-Approval
```

These gates must approve implementation scope and final destination before any
fixture, test, route, or product repo file is created or modified.
