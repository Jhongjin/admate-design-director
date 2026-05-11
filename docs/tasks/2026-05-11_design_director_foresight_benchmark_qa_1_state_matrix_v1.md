# Design Director Foresight Benchmark QA 1 State Matrix v1

Date: 2026-05-11
Gate: Design-Foresight-Benchmark-QA-1
Status: docs-only gate
Depends on:
- `docs/product-ui-direction/foresight-ui-direction-v1.md`
- `docs/prompts/foresight-ui-review-prompt-v1.md`
- `docs/reviews/design-gate-priority-map-v1.md`

Scope: define the Foresight benchmark/report data trust state matrix and
review boundaries for a future product QA gate. This gate does not change
Foresight product code, prediction logic, benchmark calculation, data pipeline,
API, DB, auth, env, screenshots, assets, or product repo files.

## Purpose

Foresight should help users judge forecast and benchmark outputs by showing the
basis behind the numbers.

The Design Director position is:

```text
Metrics do not create trust alone.
Basis, confidence range, recent-data policy, and exclusions create trust.
```

This document defines the first benchmark/report QA matrix before any product
UI implementation or screenshot audit.

## State Matrix

| State | Meaning | Required UI expectation | Failure signal |
| --- | --- | --- | --- |
| `benchmark-ready` | Aggregate benchmark data is usable for the selected platform, industry, objective, period, and metric. | Show metric, period, basis, sample/coverage, and confidence together. | KPI card shows a number without period, basis, or confidence context. |
| `low-confidence` | Data is usable but limited by sample size, coverage, variance, or stale filters. | Show confidence range and reason near the metric. | A low-confidence forecast looks as reliable as a high-confidence one. |
| `long-term-trend-only` | Data is older than the preferred recent window and should inform trend only. | Mark as long-term reference and keep separate from recent benchmark. | Old data is mixed into current benchmark without warning. |
| `validation-error` | Upload or mapping has missing or invalid required fields. | Explain row/column-level issue and next operator action. | User only sees a generic failure or silent empty chart. |
| `security-review-required` | Upload or input includes credential-bearing URL, token-like value, or raw identifier risk. | Stop promotion and show sanitized security review reason. | Sensitive value appears in UI, report, prompt, or screenshot. |
| `raw-identifier-risk` | Source contains account, campaign, ad set, ad, advertiser, or other raw identifiers. | Keep identifiers out of report-ready display and LLM/prompt boundaries. | Raw campaign-level identifiers appear in mock/report output. |
| `no-benchmark-data` | No usable aggregate data exists for selected scope. | Show no-data state with checked filters and safe next action. | UI fabricates a forecast or presents empty source shells as support. |

## Data Trust Display Matrix

Foresight benchmark/report UI should expose these trust dimensions before a
user relies on the forecast:

| Trust dimension | Display expectation |
| --- | --- |
| Platform | Visible filter or label. |
| Industry/objective | Visible filter or label, with long labels wrapping safely. |
| Period | Clear date range or data window. |
| Recent-data policy | Recent window and long-term trend-only distinction visible. |
| Metric basis | Source aggregation basis and selected metric visible. |
| Sample/coverage | Sample count or coverage label when available. |
| Confidence range | Range and confidence label shown near the metric. |
| Exclusions | Missing, rejected, stale, or out-of-window data summarized. |
| Currency/net-gross | Currency and net/gross/markup basis shown when relevant. |
| Generated report status | Mock, aggregate, draft, or reviewed state clearly labeled. |

## Benchmark Table QA

Pass criteria:

- table supports comparison by platform, industry, objective, period, and
  metric
- metric columns do not hide basis or confidence
- recent and long-term rows are visually separable
- validation/warning/error states are row-level or column-level when possible
- long Korean labels and mixed English metric names do not overflow
- raw campaign-level data is not displayed

P0 blockers:

- real advertiser, account, campaign, ad set, ad, billing, or private source
  data appears in the UI or QA evidence
- table presents stale or low-confidence data as current high-confidence data
- credential-bearing URLs or token-like values appear in UI or report output
- no-data state fabricates a forecast

P1 follow-ups:

- confidence badge copy needs clarity
- basis drawer is too deeply hidden
- mobile table strategy needs stacked/detail treatment
- long Korean labels require better clamp/wrap
- export/report metadata could be more explicit

## Forecast Panel QA

Pass criteria:

- forecast metric and confidence range appear in the same visual context
- basis and period are visible near the forecast
- low-confidence reason is visible before export/report action
- scenario filters are reflected in forecast labels
- chart is subordinate to data trust explanation

Failure examples:

- metric card implies precision beyond the available basis
- confidence is color-only
- chart dominates the screen while basis is hidden in a drawer
- long-term reference data looks like recent benchmark data

## Upload Mapping And Validation QA

Future review should confirm:

- missing required fields show field-level remediation
- mixed currency requires split or review
- token-bearing URL or credential-like field blocks promotion
- raw identifiers are masked or excluded from report-ready output
- long-term-only data is labeled trend-only
- rejected rows summary is understandable without exposing raw rows

Design QA should not change upload parsing, import, DB write, or validation
logic.

## Report And Export QA

Report/export surfaces should show:

- generated time
- benchmark basis
- period
- confidence range
- assumptions and exclusions
- mock/aggregate/reviewed status
- safe source labels

Forbidden report content:

- raw campaign-level rows
- account/campaign/ad IDs
- real advertiser names in mock examples
- token, cookie, credential, signed URL, env, provider raw payload
- overclaiming copy such as "정확한 예측 보장" or "성과 보장"

## Mobile QA Checklist

Future mobile review should verify:

- scenario filters wrap or collapse without covering action buttons
- metric cards stay limited and include confidence/basis links
- benchmark table has a stable horizontal scroll, stacked row, or detail drawer
  strategy
- long Korean basis explanations do not overflow
- export/report action is reachable after metadata review
- chips and badges do not truncate critical confidence labels

## Product Agent Prompt

Use this prompt for the next Foresight product QA handoff:

```text
You are the AdMate Foresight product Agent.

Gate:
Foresight-Benchmark-QA-Data-Trust

Goal:
Run read-only UI QA of benchmark, forecast, upload mapping, and report/profile
surfaces with a focus on data trust.

Do not modify code in this Gate.
Do not call production APIs.
Do not run benchmark import/upload.
Do not run SQL.
Do not call Meta API, Python retrain, or external LLMs.
Do not change prediction model, data pipeline, upload parser, API contract,
DB/schema, auth, env, or cost logic.
Do not expose raw campaign-level data, real advertiser names, account
information, credentials, tokens, cookies, or provider raw payloads.

Required review:
- benchmark-ready state
- low-confidence state
- long-term-trend-only state
- validation-error state
- security-review-required state
- no-benchmark-data state
- desktop table and forecast layout
- mobile filter/table/report behavior
- report/export metadata clarity

Report:
- P0 blockers
- P1 follow-ups
- raw-data no-exposure confirmation
- data trust display matrix result
- mobile text/table stability result
- next implementation gate recommendation
```

## No-Touch Confirmation

This gate does not perform:

- Foresight product code changes
- screenshot capture
- media or asset creation
- benchmark import/upload
- SQL execution
- DB/Auth mutation
- Meta API call
- Python retrain
- external LLM call
- production API call
- environment change
- secret, env, token, cookie, session, credential, signed URL, raw provider
  response, or raw campaign data output

## Next Gate

Recommended next gate:

```text
Gate Foresight-Benchmark-QA-Data-Trust
```

Suggested purpose:

```text
Have the Foresight product Agent perform read-only UI QA using mock or
aggregate-only evidence. Keep implementation, imports/uploads, production API
calls, SQL, Meta/Python execution, and raw campaign data out of scope.
```
