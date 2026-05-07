# Foresight UI Review Prompt v1

작성일: 2026-05-07
Gate: Design-Director-2
대상: AdMate Foresight 제품 Agent
범위: UI review, screenshot audit, prompt/rules 전달용 템플릿. 제품 repo 코드 수정은 별도 승인 전 금지한다.

---

## 1. UI 목적

Foresight는 과거 광고 데이터와 시장 트렌드를 기반으로 다음 캠페인의 예상 성과와 media planning 기준을 제공하는 제품이다.

Foresight UI의 목적은 예측 숫자를 예쁘게 보여주는 것이 아니라, benchmark basis, confidence range, 최근 데이터 기준, report/export 흐름을 신뢰 가능하게 설명하는 것이다.

핵심 기준:

```text
Foresight는 다음 성과를 예측한다.
Metric보다 basis와 confidence가 먼저 신뢰를 만든다.
Raw campaign-level data는 노출하지 않는다.
```

---

## 2. 반드시 지켜야 할 디자인 원칙

- AdMate Operational Intelligence Console 기준을 따른다.
- benchmark table과 forecast panel은 spreadsheet처럼 비교 가능해야 한다.
- confidence range, sample/basis, recent data 기준을 metric card 주변에서 확인할 수 있어야 한다.
- 최근 최대 6개월 이내 데이터 우선 기준과 6개월 초과 장기 추세 참고용을 분리한다.
- Net/Gross, markup, currency, period, filter options를 metadata로 남긴다.
- mock 또는 aggregate data만 사용한다.
- chart와 metric card가 data trust 설명을 가리면 안 된다.

---

## 3. 수정 금지 영역

제품 Agent는 아래를 수정하지 않는다.

- prediction model
- benchmark calculation logic
- data pipeline
- upload parsing logic
- cost logic
- API contract
- DB schema
- auth/session/permission
- environment variables
- raw campaign-level data
- secret, token, credential
- copied reference 원문

허용되는 범위:

- benchmark/report IA review
- table/readability review
- confidence/basis visual hierarchy review
- upload mapping UI review
- report profile UI review
- screenshot audit notes

---

## 4. 우선 점검 화면/컴포넌트

| Surface | Review Focus |
|---|---|
| Scenario filters | platform, industry, objective, period, metric 기준이 명확한가 |
| Benchmark table | metric, period, basis, sample count, coverage를 비교 가능한가 |
| Forecast panel | CPM/CPC/CTR/VTR/CPV와 confidence range가 함께 보이는가 |
| Basis drawer | recent data 기준, exclusions, sample/coverage를 설명하는가 |
| Upload mapping | column mapping, validation state, rejected rows summary가 이해되는가 |
| Scenario simulator | budget/period/objective 변경이 UI에서 과장 없이 표현되는가 |
| Report profile/export | report wording, metadata, export action이 data trust와 연결되는가 |

---

## 5. 좋은 상태 / 나쁜 상태 기준

좋은 상태:

- metric summary 옆에 basis, confidence, period, sample/coverage가 함께 보인다.
- benchmark table은 dense하지만 column label과 row 상태가 명확하다.
- 최근 6개월 기준과 장기 추세 참고용이 badge/text로 구분된다.
- upload validation error가 행/컬럼 단위로 설명된다.
- report/export 화면은 mock/aggregate 기준임을 유지한다.
- raw data 없이도 data trust 기준을 이해할 수 있다.

나쁜 상태:

- KPI 숫자와 chart가 크고 basis는 drawer 깊은 곳에 숨겨져 있다.
- confidence가 단순 green/red 색상으로만 보인다.
- 오래된 데이터와 최근 데이터가 같은 기준처럼 보인다.
- 실제 광고주명, 캠페인명, 계정정보, raw row가 mock에 들어간다.
- 예측 모델이나 pipeline을 UI 개선 중 바꾼다.
- "정확한 예측 보장"처럼 과장된 표현을 쓴다.

---

## 6. 모바일 / 데스크톱 체크 포인트

Desktop:

- filters, metric summary, benchmark table, forecast panel의 읽기 순서가 명확하다.
- table column이 너무 넓거나 좁아져 metric/basis 비교를 방해하지 않는다.
- basis drawer는 metric과 연결되어 있고 sample/exclusion 설명을 보여준다.
- export/report action은 metadata 확인 뒤 실행할 수 있다.

Mobile:

- filters가 wrap 또는 drawer로 정리되어 table action과 겹치지 않는다.
- metric card는 3-5개 이하로 묶고 confidence/basis 링크를 함께 제공한다.
- benchmark table은 horizontal scroll, stacked row, detail drawer 중 하나의 안정적인 전략을 가진다.
- 긴 report comment와 basis 설명이 card 밖으로 넘치지 않는다.

---

## 7. 긴 한국어 텍스트 안정성 기준

- report comment, basis explanation, rejected rows reason은 line-clamp와 detail 전략을 가진다.
- 업종/목표/필터명이 긴 경우 badge나 chip 안에서 줄바꿈 또는 truncation이 안정적이어야 한다.
- Net/Gross, currency, period, markup 같은 짧은 metadata는 label/value 구조로 표시한다.
- mobile에서 긴 한국어 설명이 table header나 action button을 밀어내지 않아야 한다.
- Pretext.js는 report comment나 basis 설명의 layout stability PoC 후보일 뿐 production dependency로 바로 추가하지 않는다.

---

## 8. Badge / Source / Audit / Confidence / Provenance 기준

상태 badge:

- `High confidence`: green
- `Medium confidence`: amber
- `Low confidence`: red
- `Recent data`: info/green
- `Long-term trend`: amber/neutral
- `Validation error`: red

Source:

- benchmark source는 platform, industry/objective filter, period, sample/coverage, exclusion rule로 표현한다.
- source가 raw campaign data처럼 노출되지 않게 aggregate level로만 표시한다.

Audit:

- upload/import 시점, mapping validation, rejected rows summary, report generated time을 추적 가능하게 한다.
- model/pipeline audit 자체를 UI review 범위에서 변경하지 않는다.

Confidence:

- confidence range는 metric과 같은 시야에서 확인 가능해야 한다.
- confidence 낮음은 reason과 next review action을 함께 제공한다.

Provenance:

- forecast 결과가 어떤 benchmark basis와 filter 조합에서 나왔는지 연결한다.
- mock/aggregate 기준을 사용했음을 review note에 명시한다.

---

## 9. 제품 Agent 전달용 실행 프롬프트

```text
You are reviewing AdMate Foresight UI.

Goal:
Audit the benchmark, forecast, upload mapping, and report/profile UI with a focus on data trust.

Scope:
- scenario filters
- benchmark table
- forecast panel
- confidence range display
- basis drawer
- upload mapping and validation states
- report profile/export flow
- mobile table and long Korean text behavior

Hard constraints:
- Do not modify product code unless explicitly approved in a later implementation Gate.
- Do not modify prediction models, benchmark calculation, data pipeline, upload parsing logic, cost logic, API contracts, DB schema, auth, environment, or secrets.
- Do not expose raw campaign-level data, real advertiser names, campaign names, account information, or credentials.
- Use mock or aggregated data only.
- Do not edit copied reference documents.
- Do not call external APIs.

Design direction:
- Follow AdMate Operational Intelligence Console.
- Prioritize benchmark readability, confidence range, basis/provenance, recent data policy, Korean text stability, and report/export clarity.
- Keep charts and metric cards subordinate to data trust explanation.
- Avoid overclaiming prediction accuracy.

Review output:
- benchmark/report IA audit
- P0/P1 UI issues
- data trust display matrix
- desktop and mobile table/text notes
- raw-data no-exposure confirmation
- recommended prompt/rules for a future implementation Gate
```
