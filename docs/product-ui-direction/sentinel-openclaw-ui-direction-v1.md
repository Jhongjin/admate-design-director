# Sentinel / Openclaw UI Direction v1

작성일: 2026-05-07
대상: AdMate Sentinel, Openclaw, Agent Core operations
목적: 캠페인 사고 방지와 Agent Core 운영 화면이 안전하고 추적 가능한 critical operations UI로 유지되도록 디자인 방향을 정의한다.

---

## 1. Product Role

Sentinel은 캠페인 시작 전 세팅 검수와 집행 후 실시간 운영 감지를 담당한다.

Openclaw는 Agent Core의 실행 엔진으로 workflow, 알림, Slack action, audit/operator action 기록을 담당한다.

한 줄 기준:

```text
Sentinel은 캠페인 사고를 막고 감지한다.
Openclaw는 실행하고 기록한다.
```

---

## 2. Design Objective

Sentinel/Openclaw UI의 목표:

- critical alert, warning, normal 상태를 즉시 구분한다.
- operator action과 audit history를 추적 가능하게 한다.
- suppression, snooze, stop, resume 같은 위험 action을 명확히 구분한다.
- fail-closed/security warning을 차분하지만 강하게 표시한다.
- 운영자가 실수하지 않도록 action hierarchy를 제한한다.

추천 조합:

```text
IBM Carbon + Linear + Sentry
```

AdMate 번역:

```text
enterprise 운영 구조
+ incident/status 추적성
+ workflow action clarity
```

---

## 3. Primary Surfaces

| Surface | Direction |
|---|---|
| Live monitoring dashboard | alert severity, campaign state, budget/KPI status |
| Alert timeline | reason, detected time, delivery, suppression, operator action |
| Pre-launch validation | media mix vs platform settings 비교 결과 |
| Access request / governance | 권한 요청, 승인, audit trail |
| Operator action log | 누가 어떤 action을 했는지 추적 |
| Fail-closed warning | 데이터 수집 실패와 권한 오류를 가짜 정상으로 보이지 않게 표시 |
| Cost/security panel | LLM/API 비용과 민감 action 정책 표시 |

---

## 4. Visual Direction

기본:

- dense dashboard
- compact status cards
- table/list 중심
- alert detail drawer
- audit timeline

Accent:

- core graphite / black
- normal green
- review amber
- critical red
- info indigo

권장 구조:

```text
Topbar
System/campaign status summary
Alert queue or validation queue
Selected issue detail
Operator actions
Audit/history
```

---

## 5. Critical Operation Rules

Sentinel critical operations에는 Taste Skill/image-first workflow를 직접 적용하지 않는다.

금지:

- 과한 animation
- cinematic dark incident UI
- gradient/glass warning panel
- action button hierarchy를 흐리는 장식
- monitoring threshold, suppression logic, auth/permission logic 변경

권장:

- severity badge
- explicit confirmation
- disabled state reason
- audit preview before critical action
- failure reason과 retry 가능 여부 구분

---

## 6. State Model

| State | UI Treatment |
|---|---|
| Normal | green badge, compact summary |
| Review needed | amber/info badge, reason visible |
| Critical | red badge, primary area에 표시 |
| Suppressed | neutral/amber badge, until time 표시 |
| Stopped today | amber badge, operator와 reason 표시 |
| Resumed | history에 resume action 기록 |
| Data unavailable | fail-closed state, 가짜 0값 금지 |
| Permission blocked | 권한 부족 reason과 요청 path 표시 |

---

## 7. Taste Skill Translation

Sentinel/Openclaw에는 redesign/minimalist 참고만 제한적으로 사용한다.

허용:

- access request approval UI 정리
- audit log readability 개선
- non-critical settings list 정리

직접 적용 금지:

- critical alert 판단 화면
- incident action flow
- Slack button action logic
- threshold/suppression/monitoring logic

제품 Agent 전달 prompt seed:

```text
Follow AdMate Operational Intelligence Console.
Use only restrained redesign/minimalist references for non-critical UI surfaces.
Do not apply image-first, animation-heavy, cinematic, glass, or gradient-heavy styling.
Do not modify monitoring logic, threshold logic, Slack action behavior, API contracts, DB, auth, audit logging, operator action logging, or environment.
Prioritize severity clarity, operator action traceability, fail-closed states, permission clarity, and compact incident review.
```

---

## 8. QA Focus

- critical action과 secondary action이 명확히 분리되는가?
- alert reason이 숨겨지지 않는가?
- data unavailable을 정상 상태처럼 보이게 하지 않는가?
- operator action과 audit log가 연결되는가?
- mobile에서 critical action이 accidental tap에 취약하지 않은가?
