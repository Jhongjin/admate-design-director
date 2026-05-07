# Sentinel / Agent Core UI Review Prompt v1

작성일: 2026-05-07
Gate: Design-Director-2
대상: AdMate Sentinel / Agent Core 제품 Agent
범위: UI review, screenshot audit, prompt/rules 전달용 템플릿. 제품 repo 코드 수정은 별도 승인 전 금지한다.

---

## 1. UI 목적

Sentinel은 캠페인 시작 전 세팅 검수와 집행 후 실시간 운영 감지를 담당한다.

Agent Core는 Openclaw와 Hermes 기반으로 workflow 실행, operator action, audit log, approval, governance를 연결한다.

Sentinel / Agent Core UI의 목적은 화려한 incident UI가 아니라, 운영자가 severity, reason, next action, audit trail을 빠르게 판단하고 실수 없이 처리하도록 돕는 것이다.

핵심 기준:

```text
Sentinel은 캠페인 사고를 막고 감지한다.
Agent Core는 실행하고 기록한다.
Fail-closed, audit, operator action이 critical UI의 중심이다.
```

---

## 2. 반드시 지켜야 할 디자인 원칙

- AdMate Operational Intelligence Console 기준을 따른다.
- critical, warning, normal, suppressed, data unavailable, permission blocked 상태를 명확히 구분한다.
- data unavailable은 절대 정상 또는 0값처럼 보이면 안 된다.
- operator action과 audit log는 연결되어 보여야 한다.
- suppression, snooze, stop today, resume 같은 action은 위험도와 confirmation이 분리되어야 한다.
- fail-closed/security warning은 차분하지만 강하게 표시한다.
- motion, decorative warning panel, cinematic dark, glass/gradient-heavy incident UI를 쓰지 않는다.

---

## 3. 수정 금지 영역

제품 Agent는 아래를 수정하지 않는다.

- monitoring threshold logic
- suppression/snooze/stop/resume logic
- Slack action behavior
- alert detection logic
- validation comparison logic
- Hermes learning logic
- audit logging logic
- operator action logging logic
- API contract
- DB schema
- auth/session/permission
- environment variables
- secret, token, credential
- copied reference 원문

허용되는 범위:

- alert/validation UI review
- approval/audit readability review
- action hierarchy review
- fail-closed state wording/layout review
- permission blocked UI review
- screenshot audit notes

---

## 4. 우선 점검 화면/컴포넌트

| Surface | Review Focus |
|---|---|
| Live monitoring dashboard | severity, campaign state, budget/KPI status가 즉시 보이는가 |
| Alert queue/detail | reason, detected time, affected campaign context, next action이 명확한가 |
| Pre-launch validation | media mix vs platform setting 차이가 비교 가능한가 |
| Approval queue | request, approver, status, risk note, action이 분리되는가 |
| Operator action log | 누가, 언제, 어떤 action을 했는지 보이는가 |
| Audit timeline | alert, action, permission, delivery 기록이 연결되는가 |
| Fail-closed warning | data unavailable과 permission blocked가 정상처럼 보이지 않는가 |
| Cost/security panel | 민감 action과 비용/보안 상태가 과장 없이 보이는가 |

---

## 5. 좋은 상태 / 나쁜 상태 기준

좋은 상태:

- severity badge와 reason이 첫 viewport에서 보인다.
- critical action은 primary처럼 보이더라도 confirmation과 disabled reason을 가진다.
- suppressed/stopped/resumed 상태는 until time, operator, reason을 함께 보여준다.
- data unavailable은 fail-closed copy와 recovery path를 제공한다.
- audit timeline에서 alert, notification, operator action, result가 연결된다.
- mobile에서 accidental tap 위험이 낮다.

나쁜 상태:

- data unavailable이 정상 0값 또는 normal badge처럼 보인다.
- suppression, stop today, resume action이 같은 버튼 hierarchy로 보인다.
- alert reason이 drawer 깊은 곳에 숨겨져 있다.
- audit log가 raw JSON/debug dump처럼 노출된다.
- motion이나 decoration이 severity 판단보다 강하다.
- monitoring/auth/audit logic을 UI 개선 중 바꾼다.

---

## 6. 모바일 / 데스크톱 체크 포인트

Desktop:

- system/campaign status summary, alert queue, selected issue detail, operator actions, audit history가 분리된다.
- dense table/list에서도 severity와 reason이 잘린 채 방치되지 않는다.
- critical action button은 confirmation preview 또는 reason checklist를 가진다.
- audit timeline은 timestamp, actor, action, result가 스캔 가능하다.

Mobile:

- critical action은 thumb zone에 있어도 accidental tap 방지 장치를 가진다.
- severity badge, reason, next action이 한 화면에서 확인 가능하다.
- alert detail drawer 또는 accordion이 action button을 가리지 않는다.
- 긴 alert reason과 operator note가 overflow되지 않는다.

---

## 7. 긴 한국어 텍스트 안정성 기준

- alert reason, validation mismatch reason, operator note는 line-clamp와 detail 전략을 가진다.
- critical copy는 짧고 명확해야 하며, 긴 설명은 detail panel로 이동한다.
- button label은 `보류`, `오늘 중지`, `재개`, `승인`, `거절`처럼 짧은 동사형을 우선한다.
- campaign/account/client 실제명은 예시로 쓰지 않고 mock label 또는 anonymized label만 사용한다.
- mobile에서 reason text가 action buttons와 겹치지 않아야 한다.

---

## 8. Badge / Source / Audit / Confidence / Provenance 기준

상태 badge:

- `Normal`: green
- `Review needed`: amber/info
- `Critical`: red
- `Suppressed`: neutral/amber with until time
- `Stopped today`: amber with operator/reason
- `Resumed`: history state
- `Data unavailable`: red/amber fail-closed
- `Permission blocked`: red/amber with request path

Source:

- alert source는 detection reason, metric, threshold label, detected time, data freshness로 표현한다.
- validation source는 media mix reference와 platform setting snapshot의 비교 근거를 표시한다.

Audit:

- actor, timestamp, action, reason, result, delivery status를 연결한다.
- audit log는 숨기지 않되 raw debug data로 기본 노출하지 않는다.

Confidence:

- monitoring result를 확정처럼 보이게 하기보다 data freshness와 unavailable state를 분리한다.
- confidence가 불완전하면 review needed 또는 fail-closed로 표시한다.

Provenance:

- alert/validation/action이 어떤 source snapshot과 operator action에서 비롯됐는지 추적 가능해야 한다.
- Hermes 학습 반영 여부는 권한과 approval 상태 없이는 승인된 기준처럼 보이지 않게 한다.

---

## 9. 제품 Agent 전달용 실행 프롬프트

```text
You are reviewing AdMate Sentinel / Agent Core UI.

Goal:
Audit critical operations UI with a focus on fail-closed states, audit traceability, operator action clarity, and permission clarity.

Scope:
- live monitoring dashboard
- alert queue and detail
- pre-launch validation comparison
- approval queue
- operator action log
- audit timeline
- fail-closed and permission blocked states
- desktop and mobile critical action behavior

Hard constraints:
- Do not modify product code unless explicitly approved in a later implementation Gate.
- Do not modify monitoring thresholds, suppression logic, Slack action behavior, alert detection, validation comparison, Hermes learning, audit logging, operator action logging, API contracts, DB schema, auth, environment, or secrets.
- Do not edit copied reference documents.
- Do not call external APIs.
- Do not use real campaign names, advertiser names, account information, raw campaign-level data, tokens, or credentials.

Design direction:
- Follow AdMate Operational Intelligence Console.
- Prioritize severity clarity, fail-closed states, operator action traceability, audit readability, permission clarity, and compact incident review.
- Avoid animation-heavy, image-first, cinematic dark, glass, or gradient-heavy incident styling.
- Never make data unavailable look like normal or zero.

Review output:
- approval/audit readability audit
- P0/P1 UI issues
- critical action hierarchy note
- fail-closed state wording matrix
- desktop and mobile accidental tap notes
- no-touch areas confirmed
- recommended prompt/rules for a future implementation Gate
```
