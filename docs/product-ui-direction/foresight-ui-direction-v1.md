# Foresight UI Direction v1

작성일: 2026-05-07
대상: AdMate Foresight, Media Planning Intelligence
목적: Foresight가 예측/벤치마크/리포트 UI를 신뢰형 analytics workspace로 제공하도록 제품별 디자인 방향을 정의한다.

---

## 1. Product Role

Foresight는 과거 광고 데이터와 시장 트렌드를 기반으로 다음 캠페인의 예상 성과를 제공한다.

한 줄 기준:

```text
Foresight는 다음 성과를 예측한다.
```

Foresight UI는 "숫자를 예쁘게 보여주는 dashboard"가 아니라 "어떤 기준으로 어느 범위까지 믿을 수 있는지"를 설명해야 한다.

---

## 2. Design Objective

Foresight의 디자인 목표:

- benchmark table과 forecast panel을 빠르게 비교할 수 있게 한다.
- confidence range, data basis, recent data 기준을 분리해 보여준다.
- upload mapping flow와 report profile을 실무형으로 정리한다.
- raw campaign-level data를 노출하지 않는다.

추천 조합:

```text
Airtable + IBM Carbon + Linear
```

AdMate 번역:

```text
spreadsheet의 비교성
+ enterprise analytics의 근거 구조
+ workflow UI의 action clarity
```

---

## 3. Primary Surfaces

| Surface | Direction |
|---|---|
| Benchmark table | platform, industry, objective, metric, period, basis를 비교 |
| Forecast panel | 예상 CPM/CPC/CTR/VTR/CPV와 confidence range 표시 |
| Upload mapping | 컬럼 매핑, validation state, rejected rows summary |
| Scenario simulator | budget/period/objective 변경에 따른 예측 범위 |
| Report profile | 광고주/업종별 리포트 표현 선호를 template화 |
| Basis drawer | 최근 데이터 기준, sample count, exclusions 표시 |

---

## 4. Visual Direction

기본:

- neutral background
- compact table/list
- metric card는 3-5개 이하로 묶는다.
- chart는 근거 이해를 돕는 최소 형태로 사용한다.

Accent:

- Foresight accent: amber
- analytic secondary: blue
- confidence high: green
- confidence medium: amber
- confidence low: red

권장 구조:

```text
Topbar
Scenario filters
Metric summary
Benchmark table
Forecast panel
Basis / confidence drawer
Report profile actions
```

---

## 5. Data Trust Rules

Foresight UI는 데이터 기준을 숨기면 안 된다.

표시해야 할 정보:

- 조회 시점
- platform
- industry/objective filter
- 기간
- 최근 데이터 기준
- sample count 또는 coverage 수준
- confidence range
- Net/Gross, markup, currency 기준
- 제외/보류 데이터 조건

기본 원칙:

```text
최근 최대 6개월 이내 데이터를 우선한다.
6개월 초과 데이터는 장기 추세 참고용으로 분리한다.
raw campaign-level data는 LLM prompt나 UI 예시에 직접 노출하지 않는다.
```

---

## 6. Taste Skill Translation

Foresight에는 minimalist/soft 방향만 제한적으로 적용한다.

허용:

- soft-skill reference for report UI
- minimalist-skill reference for benchmark workspace
- imagegen-frontend-web reference for non-production report concept

제한:

- table readability가 motion/decoration보다 우선이다.
- 실제 raw campaign data를 prompt나 mock에 넣지 않는다.
- 예측 모델, DB, pipeline, cost logic은 디자인 Gate 범위 밖이다.

제품 Agent 전달 prompt seed:

```text
Use minimalist/soft taste direction only as a reference.
Follow AdMate Operational Intelligence Console.
Redesign only Foresight benchmark table, forecast panel, upload mapping flow, and report profile UI.
Do not modify prediction model, DB schema, data pipeline, API contracts, auth, environment, or cost logic.
Use mock or aggregated data only. Do not expose raw campaign-level data.
Prioritize table readability, confidence range, benchmark basis, Korean text stability, and export/report workflow clarity.
```

---

## 7. QA Focus

- benchmark table이 한 화면에서 비교 가능한가?
- confidence와 basis가 metric보다 숨겨지지 않는가?
- upload validation error가 행/컬럼 단위로 이해 가능한가?
- raw data 노출 없이 aggregate/mock 기준으로 설명되는가?
- mobile에서 filter와 table action이 겹치지 않는가?
