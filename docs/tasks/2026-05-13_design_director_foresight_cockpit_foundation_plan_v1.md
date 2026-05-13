# Design Director Foresight Cockpit Foundation Plan v1

Date: 2026-05-13
Gate: Design-Director-Foresight-Cockpit-Foundation-Plan-1
Status: Draft, docs-only

## Scope

This plan translates the AdMate product cockpit direction into a Foresight-specific
implementation plan. It does not implement product code.

No code changes, SQL execution, DB/Auth mutation, production API calls, Vercel
configuration changes, environment changes, product repo commits, secret readback,
or credential inspection were performed in this gate.

## Inputs

- `docs/tasks/2026-05-13_design_director_admate_product_taste_strategy_v1.md`
- `docs/tasks/2026-05-13_design_director_admate_product_cockpit_contracts_v1.md`
- Foresight authenticated production smoke screenshots from 2026-05-13
- Read-only Foresight repo inventory
- Commander sub-agent review notes for Foresight implementation surface and UI risk

## Current Foresight Observation

Foresight is functional enough for authenticated use after Core handoff. The current
production surfaces show:

- `/trends` is protected and reachable after login.
- Empty trend charts are clear but visually sparse.
- Simulator and trend surfaces use clean cards and restrained UI.
- The product still reads more like a simple tool screen than a planning cockpit.
- Logout returns users to the Foresight login shell as expected.

This gate should not reinterpret auth, handoff, benchmark logic, prediction math, or
data contracts. The design work should expose the existing trust model more clearly.

## Product Role

Foresight should present itself as a media planning cockpit.

The first viewport should answer:

- What planning question am I answering?
- Which campaign inputs are driving the forecast?
- Is benchmark support available for this selection?
- Which output is ready, limited, blocked, or not yet trustworthy?
- What should the media planner do next?

Foresight should not present itself as a generic AI dashboard, creative showcase, or
decorative analytics landing page.

## Implementation Surfaces

Primary Foresight repo files:

- `D:\Projects\AdMate\admate-foresight\app\SimulatorPageClient.tsx`
- `D:\Projects\AdMate\admate-foresight\app\trends\TrendsPageClient.tsx`
- `D:\Projects\AdMate\admate-foresight\components\KPICard.tsx`
- `D:\Projects\AdMate\admate-foresight\components\StatePanel.tsx`
- `D:\Projects\AdMate\admate-foresight\components\Navigation.tsx`
- `D:\Projects\AdMate\admate-foresight\lib\benchmark\uiStateViewModel.ts`

The initial implementation should prefer component composition and copy/layout
changes over new data contracts.

## No-Touch Surfaces

Do not change:

- Auth, handoff, login, logout, account, or session guard behavior.
- `/api/*` route contracts.
- Predictor math, model loading, benchmark import, Meta sync, Python retrain, DB
  schema, migrations, or environment variables.
- Benchmark governance semantics such as recent-period basis, sample count, currency,
  net/gross interpretation, synthetic context, or security review states.
- Raw user identifiers, tokens, cookies, sessions, credentials, provider payloads, or
  environment values.

## Cockpit Information Architecture

Recommended first implementation order:

1. Planning header
   - Keep the product identity visible.
   - State the planner task in operational language.
   - Avoid hero marketing composition.

2. Campaign setup rail
   - Keep existing budget, duration, industry, objective, gender, age, and seasonality
     controls.
   - Make the primary forecast action visually stable and reachable near the top on
     mobile.

3. Forecast readiness panel
   - Show whether Foresight can forecast now, needs more inputs, is loading, or is
     blocked by trust conditions.
   - Surface the reason before the user reads empty charts.

4. KPI forecast cards
   - Preserve existing KPI logic.
   - Use compact comparison cards for expected CPM, CPC, CTR, reach, or confidence
     where data is available.

5. Benchmark trust panel
   - Use the existing benchmark view model states.
   - Show confidence, data basis, sample availability, synthetic context, and blocked
     output status without exposing raw data.

6. Optimization guidance
   - Translate forecast state into planner next actions.
   - Examples: widen filters, select a comparable industry, switch to long-term trend,
     or review security state.

7. Trends surfaces
   - Keep `/trends` as a benchmark and trend detail surface.
   - Empty states should explain what is missing and how to make the chart useful.

8. ML comparison
   - Keep ML comparison separate from benchmark-backed forecast.
   - Label it as exploratory when trust is lower.

## Required State Model

The UI should clearly distinguish these states:

- `preRun`: inputs are ready but no forecast has been requested.
- `loading`: forecast or benchmark data is being calculated.
- `benchmark-ready`: benchmark support is available.
- `no-benchmark-data`: no matching benchmark data exists for current filters.
- `low-confidence`: output can be shown only with caution.
- `long-term-trend-only`: recent benchmark is unavailable, but long-term trend can be
  used as a fallback.
- `validation-error`: required inputs are missing or invalid.
- `security-review-required`: output is blocked by policy or privacy review.
- `raw-identifier-risk`: output is blocked because raw identifiers would be exposed.
- `auth-expired`: user must re-enter through the login flow.
- `export-unavailable`: report/export is not ready.

## Copy Principles

Copy should be specific, calm, and operational.

Preferred:

- "최근 6개월 기준 벤치마크가 없습니다. 장기 추세 참고값으로 전환하거나 필터를 넓혀 주세요."
- "선택한 업종과 목표 조합은 표본이 부족합니다. 예측은 보류하고 비교 조건을 조정하세요."
- "보안 검토가 필요한 데이터가 포함되어 있어 보고서 출력을 제한했습니다."

Avoid:

- "AI가 분석 중입니다."
- "데이터가 없습니다." as a standalone explanation.
- Claims that imply guaranteed campaign performance.
- Decorative or vague expertise language.

## Visual Direction

Foresight should feel like a professional media planning console:

- Light neutral base.
- Dense but breathable information hierarchy.
- Restrained accent color for selected metric, readiness, and next action.
- Table/chart/status composition over hero imagery.
- Compact cards with stable dimensions.
- No generic dark AI gradient, decorative orb, oversized hero block, or illustrative
  landing page.

Foresight can keep a distinct identity from Compass, Lens, and Sentinel, but it should
share the AdMate cockpit grammar: status, evidence, action, and operational trust.

## Mobile Requirements

Mobile layout order:

1. Product header and menu.
2. Planning task title.
3. Campaign setup summary and primary action.
4. Forecast readiness.
5. Key KPI cards.
6. Benchmark trust explanation.
7. Optimization guidance.
8. Charts and trend details.

Mobile acceptance criteria:

- No horizontal overflow at 360, 390, 430, and tablet widths.
- Primary action remains reachable without scanning the full page.
- Empty chart states do not dominate the first viewport without explanation.
- Long Korean labels wrap cleanly.
- Select controls and cards keep stable heights.
- Footer call-to-action does not trap the user below blank charts.

## Verification Plan

For the implementation gate, run:

- `npm run lint`
- `npm run build`
- `npm run benchmark:dry-run`
- `npm run check:auth-handoff-static`
- `npm run check:protected-error-states`
- `npm run check:benchmark-kpi-static-contract`
- `npm run test:benchmark-ui`
- `npm run benchmark:ui-fixtures`

Browser smoke:

- Authenticated `/`
- Authenticated `/trends`
- Authenticated `/account`
- Logout then protected page redirect
- Desktop 1440x900
- Mobile 390x844

No production API mutation, Meta API call, Python retrain, benchmark import/upload, SQL,
or DB/Auth mutation should be part of this design implementation gate.

## Rollout Order

1. This design foundation plan.
2. Foresight cockpit implementation patch.
3. Local visual and static verification.
4. Commit/push after target-file review.
5. Post-deploy authenticated smoke only after separate human approval.
6. Design Director visual review.

## Next Gate

Recommended next gate:

`Foresight-Cockpit-Foundation-Implementation-1`

Goal: Implement the cockpit-first Foresight simulator and trend empty-state foundation
without changing auth, API, benchmark math, DB, or production data behavior.
