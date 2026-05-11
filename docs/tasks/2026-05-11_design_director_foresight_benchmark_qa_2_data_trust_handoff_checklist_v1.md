# Design Director Foresight Benchmark QA 2 Data Trust Handoff Checklist v1

Date: 2026-05-11
Gate: Design-Foresight-Benchmark-QA-2
Status: docs-only gate
Depends on:
- `docs/tasks/2026-05-11_design_director_foresight_benchmark_qa_1_state_matrix_v1.md`

Scope: translate the Foresight benchmark state matrix into a handoff checklist
for future product QA and implementation work. This gate does not modify
Foresight product code, benchmark calculation, import pipelines, API, DB, auth,
env, screenshots, generated media, assets, or product repo files.

## Purpose

Foresight benchmark and forecast UI should be reviewed through a data-trust
lens before visual polish.

This checklist gives future implementers and reviewers a compact handoff:

```text
Every metric needs basis.
Every forecast needs confidence.
Every no-data state needs an honest boundary.
```

## Handoff Checklist

### Benchmark-ready

- metric value is visible
- platform, industry, objective, period, and selected metric are visible
- sample or coverage basis is present
- confidence or reliability label is present
- currency and net/gross basis are visible when relevant
- export/report action preserves the same basis labels

### Low-confidence

- low-confidence state is visible near the metric
- reason is shown in text, not only color
- forecast or benchmark action remains cautious
- chart does not imply precision beyond the basis
- export/report includes the low-confidence reason

### Long-term-trend-only

- long-term source is separated from recent benchmark data
- trend-only label is visible before reuse/export
- recent-data policy is clear
- default benchmark cards do not mix long-term rows into current benchmarks

### Validation-error

- missing field or invalid mapping is named at field/column level
- next operator action is explicit
- no generic failure replaces actionable remediation
- raw row data is not exposed in QA evidence

### Security-review-required

- credential-like or private-source risk blocks promotion
- sanitized security reason is visible
- raw values are not shown in UI, logs, prompt copy, export, or screenshot
- reviewer action is explicit and blocking

### Raw-identifier-risk

- raw account, campaign, ad set, ad, advertiser, or tenant identifiers are
  excluded from report-ready output
- aggregate-safe preview remains available only if safe
- UI copy explains why identifiers are review-only

### No-benchmark-data

- no-data state names the checked scope
- UI does not fabricate a forecast
- safe next action is visible
- empty source or confidence shells are not shown as evidence

## Review Surface Checklist

Future product QA should inspect:

- KPI cards
- forecast panel
- benchmark table
- upload validation panel
- generated report preview
- export metadata
- mobile stacked/detail view
- empty, warning, blocked, and deprecated data states

## Copy Rules

Use:

- "최근 기준 데이터가 부족합니다."
- "장기 추세 참고용입니다."
- "검토가 필요한 원본 식별자가 포함되어 있습니다."
- "선택한 조건의 벤치마크 근거가 없습니다."

Avoid:

- "정확한 예측"
- "확실한 성과"
- "데이터 없음" without scope or next action
- technical parser or table names in user-facing copy

## Evidence Rules

QA evidence may include sanitized screenshots of UI states only when:

- no raw advertiser, account, campaign, billing, token, credential, private
  URL, or raw row detail is visible
- mock or fixture labels are clearly synthetic
- confidence and basis labels remain readable
- mobile screenshots do not hide the state label or next action

## No-Touch Confirmation

This gate does not perform:

- Foresight repo edits
- benchmark import or upload
- SQL execution
- DB/schema changes
- Meta API calls
- Python retrain
- external LLM calls
- production traffic
- screenshot capture
- asset or generated media creation
- secret, env, token, cookie, session, credential, signed URL, raw provider, or
  raw row output

## Next Gate

Recommended next gate:

```text
Gate Foresight-Benchmark-QA-2 local UI state fixture plan
```

That gate should decide whether product-side local fixture UI checks can be
defined without importing benchmark data or calling production services.
