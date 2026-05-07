# Homepage / Command Center UI Review Prompt v1

작성일: 2026-05-07
Gate: Design-Director-2
대상: AdMate Homepage / Command Center 제품 Agent
범위: UI review, screenshot audit, prompt/rules 전달용 템플릿. 제품 repo 코드 수정은 별도 승인 전 금지한다.

---

## 1. UI 목적

Homepage는 AdMate의 제품군과 브랜드 메시지를 전달하는 대표 surface다.

Command Center는 AdMate 제품군의 진행 상태와 운영 지표를 executive 관점에서 읽는 read-only console이다.

이 UI의 목적은 AdMate가 광고 운영 생애주기 전체를 연결하는 AI Agent 플랫폼임을 명확히 전달하면서, 내부 링크나 민감 action을 노출하지 않는 것이다.

핵심 기준:

```text
Homepage는 브랜드와 제품군 관계를 설명한다.
Command Center는 executive console로 현재 상태와 근거를 보여준다.
내부 운영 action과 민감 링크는 노출하지 않는다.
```

---

## 2. 반드시 지켜야 할 디자인 원칙

- AdMate Operational Intelligence Console 기준을 유지하되 Homepage는 제한적으로 브랜드/스토리텔링을 허용한다.
- first viewport에서 AdMate와 "AI Agent 기반 광고 운영 자동화 플랫폼" 정체성이 보여야 한다.
- Compass, Sentinel, Lens, Foresight, Agent Core의 관계를 하나의 campaign lifecycle로 설명한다.
- Command Center는 read-only executive dashboard처럼 보여야 한다.
- product accent는 muted하게 사용하고 전체 neutral system을 침범하지 않는다.
- internal admin link, write action, approval action, secret-like value를 public/read-only surface에 노출하지 않는다.
- 구현 수준을 실제보다 높게 보이게 하는 과장 copy를 쓰지 않는다.

---

## 3. 수정 금지 영역

제품 Agent는 아래를 수정하지 않는다.

- auth/session/permission
- API contract
- DB schema
- environment variables
- internal admin routes
- production deployment settings
- analytics/cost logic
- secret, token, credential
- 실제 campaign raw data
- 실제 광고주명, 캠페인명, 계정정보
- copied reference 원문

허용되는 범위:

- homepage IA review
- hero/product card/lifecycle section review
- CTA exposure review
- Command Center read-only dashboard review
- product maturity wording review
- screenshot audit notes

---

## 4. 우선 점검 화면/컴포넌트

| Surface | Review Focus |
|---|---|
| Hero | AdMate 정체성과 offer가 첫 viewport에서 명확한가 |
| Product cards | Compass/Sentinel/Lens/Foresight/Agent Core 관계가 통일되어 보이는가 |
| Campaign lifecycle | 기획, 정책 확인, 검수, 운영, 캡처, 평가, 학습 흐름이 이어지는가 |
| Agent Core section | Openclaw/Hermes가 내부 engine으로 설명되는가 |
| CTA | public/executive/internal 노출 범위가 분리되는가 |
| Command Center summary | read-only status, last updated, source/basis가 보이는가 |
| Product progress cards | 제품별 상태, owner, next milestone, evidence가 과장 없이 보이는가 |
| Cost / Intelligence panels | 비용/기술 변화/운영 상태가 aggregate/mock 기준으로 보이는가 |

---

## 5. 좋은 상태 / 나쁜 상태 기준

좋은 상태:

- Hero에서 AdMate가 광고 운영 Agent 플랫폼임을 바로 이해할 수 있다.
- 제품 카드는 하나의 운영 생애주기 안에 배치된다.
- Agent Core는 외부 독립 제품처럼 과장되지 않고 공통 실행/기억 레이어로 설명된다.
- Command Center card는 status, basis, last updated, next action/read-only note를 가진다.
- CTA와 link는 public-safe 또는 read-only 성격이 분명하다.
- mock/aggregate 지표는 실제 운영 데이터처럼 오해되지 않는다.

나쁜 상태:

- landing page가 generic AI SaaS hero처럼 보이고 운영 콘솔 신뢰감이 약하다.
- product card accent가 너무 강해 제품들이 별도 서비스처럼 보인다.
- 내부 admin route, 신청/승인/write action이 public surface에 노출된다.
- Openclaw/Hermes를 외부 판매 제품처럼 설명한다.
- 실제보다 높은 구현 성숙도 또는 production guarantee를 암시한다.
- 실제 광고주/캠페인/계정 정보가 예시로 들어간다.

---

## 6. 모바일 / 데스크톱 체크 포인트

Desktop:

- hero, product ecosystem, lifecycle, Agent Core, Command Center 흐름이 한 번에 스캔된다.
- product cards는 같은 density와 badge 규칙을 따른다.
- executive dashboard는 table/card hierarchy가 안정적이고 read-only action만 가진다.
- CTA가 내부 운영 action처럼 보이지 않는다.

Mobile:

- hero text가 viewport width로 font-size를 스케일하지 않고 자연스럽게 줄바꿈된다.
- product cards가 너무 긴 copy로 card 높이를 과도하게 늘리지 않는다.
- lifecycle steps가 가로 scroll에만 의존하지 않고 읽힌다.
- Command Center cards의 status, last updated, source/basis가 겹치지 않는다.

---

## 7. 긴 한국어 텍스트 안정성 기준

- hero supporting copy는 2-3문장 이하로 유지하고 긴 설명은 section body로 보낸다.
- product card 설명은 짧은 역할 문장과 핵심 bullets로 제한한다.
- CTA button은 짧은 한국어 동사형 또는 명사형을 사용한다.
- mobile에서 product name, status badge, updated time이 한 줄에 억지로 들어가지 않게 한다.
- executive copy는 과장 표현보다 상태, 근거, next milestone 중심으로 쓴다.

---

## 8. Badge / Source / Audit / Confidence / Provenance 기준

상태 badge:

- `Live`: green, 실제 확인된 경우에만
- `In progress`: info
- `Design review`: amber/info
- `Planned`: neutral
- `Blocked`: red/amber
- `Read-only`: gray/info

Source:

- Command Center의 KPI/status는 source, basis, last updated를 표시한다.
- public Homepage에는 내부 source path나 admin URL을 노출하지 않는다.

Audit:

- executive console은 write audit action을 제공하지 않더라도 status update 기준과 timestamp를 보여준다.
- 내부 승인/관리 action은 user-facing surface에 노출하지 않는다.

Confidence:

- 제품 성숙도와 운영 상태는 confidence처럼 오해되지 않게 status/basis로 설명한다.
- roadmap 또는 planned item은 live처럼 보이지 않게 한다.

Provenance:

- product status, lifecycle claim, Agent Core 설명은 중앙 문서 기준과 연결된다.
- mock/aggregate/dashboard placeholder는 실제 운영 지표처럼 보이지 않게 labeling한다.

---

## 9. 제품 Agent 전달용 실행 프롬프트

```text
You are reviewing AdMate Homepage / Command Center UI.

Goal:
Audit the Homepage and Command Center as brand plus executive read-only surfaces for the AdMate platform.

Scope:
- homepage hero
- product ecosystem cards
- campaign lifecycle section
- Agent Core section
- CTA exposure
- Command Center summary
- product progress cards
- executive status/source/basis panels
- desktop and mobile Korean text behavior

Hard constraints:
- Do not modify product code unless explicitly approved in a later implementation Gate.
- Do not modify auth, API contracts, DB schema, environment, deployment settings, analytics/cost logic, internal routes, or secrets.
- Do not expose internal admin links, write actions, approval actions, real advertiser names, campaign names, account information, raw campaign-level data, tokens, or credentials.
- Do not edit copied reference documents.
- Do not call external APIs.

Design direction:
- Follow AdMate Operational Intelligence Console with restrained brand storytelling.
- Make AdMate's identity as an AI Agent-based advertising operations platform clear in the first viewport.
- Show Compass, Sentinel, Lens, Foresight, and Agent Core as one connected lifecycle.
- Keep Command Center read-only, executive, evidence-based, and calm.
- Avoid overclaiming product maturity, generic AI SaaS styling, cinematic dark, glassmorphism, and gradient-heavy visual language.

Review output:
- Homepage/Command Center screenshot audit
- P0/P1 UI issues
- CTA exposure matrix
- product card hierarchy notes
- Command Center read-only checklist
- desktop and mobile text overflow notes
- internal-link no-exposure confirmation
- recommended prompt/rules for a future implementation Gate
```
