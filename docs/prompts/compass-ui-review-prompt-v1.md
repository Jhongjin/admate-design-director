# Compass UI Review Prompt v1

작성일: 2026-05-07
Gate: Design-Director-2
대상: AdMate Compass 제품 Agent
범위: UI review, screenshot audit, prompt/rules 전달용 템플릿. 제품 repo 코드 수정은 별도 승인 전 금지한다.

---

## 1. UI 목적

Compass는 광고 플랫폼 정책과 가이드를 근거 기반으로 답변하는 Policy Intelligence 제품이다.

Compass UI의 목적은 사용자가 "AI가 답했다"가 아니라 "어떤 source를 근거로 어느 정도 신뢰할 수 있는 답변인지" 판단하게 만드는 것이다.

핵심 기준:

```text
Compass는 정책을 답한다.
Source와 evidence가 답변 신뢰도를 결정한다.
```

---

## 2. 반드시 지켜야 할 디자인 원칙

- AdMate Operational Intelligence Console 기준을 따른다.
- answer card보다 source/evidence/citation/confidence가 약해 보이면 안 된다.
- noDataFound, limited evidence, sources found but generation failed 상태를 분리한다.
- 긴 한국어/영어 혼합 정책 문장과 URL이 mobile에서 넘치지 않아야 한다.
- source freshness, collected time, platform, policy domain을 추적 가능하게 표시한다.
- color만으로 confidence나 risk를 전달하지 않고 badge/text를 함께 쓴다.
- marketing hero, cinematic dark, glassmorphism, decorative blob, gradient-heavy AI SaaS 스타일을 쓰지 않는다.

---

## 3. 수정 금지 영역

제품 Agent는 아래를 수정하지 않는다.

- RAG retrieval logic
- crawler/ingestion logic
- embedding, reranker, model routing
- answer generation prompt logic
- source ranking, confidence calculation
- API contract
- DB schema
- auth/session/permission
- environment variables
- secret, token, credential
- copied reference 원문

허용되는 범위:

- layout
- spacing
- typography
- answer/source visual hierarchy
- badge/state treatment
- empty/error copy
- accessibility/focus state
- screenshot audit notes

---

## 4. 우선 점검 화면/컴포넌트

| Surface | Review Focus |
|---|---|
| Question input | platform, policy domain, query context가 명확한가 |
| Answer summary | 답변과 근거 수준이 함께 보이는가 |
| Source list | verified, partial, stale, unavailable 상태가 구분되는가 |
| Source drawer/detail | excerpt, URL, collected time, source quality가 추적 가능한가 |
| Empty/error state | noDataFound와 generation failure가 분리되는가 |
| Recent questions/history | source freshness와 질문 맥락이 과도하게 숨겨지지 않는가 |
| Mobile source panel | 긴 excerpt, URL, badge가 겹치지 않는가 |

---

## 5. 좋은 상태 / 나쁜 상태 기준

좋은 상태:

- answer 옆 또는 아래에서 source evidence를 즉시 확인할 수 있다.
- verified source와 limited evidence가 badge와 설명으로 구분된다.
- source excerpt가 줄바꿈, line-clamp, drawer detail 전략을 가진다.
- noDataFound는 "검색 범위 밖" 또는 "근거 없음"으로 차분하게 안내된다.
- sources found but generation failed 상태에서 source는 계속 볼 수 있다.
- confidence/risk가 색상, badge, short text로 함께 표현된다.

나쁜 상태:

- 답변만 크고 source/citation은 보조 장식처럼 보인다.
- noDataFound, API error, generation failed가 같은 error copy로 보인다.
- source URL 또는 긴 한국어 정책 문장이 card 밖으로 넘친다.
- confidence가 색상 하나로만 표시된다.
- "정책상 안전"처럼 확정적 표현을 source 없이 사용한다.
- source ranking이나 confidence logic을 UI 작업 중 바꾼다.

---

## 6. 모바일 / 데스크톱 체크 포인트

Desktop:

- answer, source panel, follow-up action이 한 화면에서 읽힌다.
- source drawer가 answer를 완전히 가리지 않고 상세 근거를 보여준다.
- table/list density가 낮아져 source 확인 횟수가 늘어나지 않는다.
- keyboard focus가 question input, source card, drawer close, copy/open link에 도달한다.

Mobile:

- question input과 primary action이 겹치지 않는다.
- source card badge, URL, excerpt가 줄바꿈되거나 detail로 이동한다.
- drawer 또는 accordion이 화면 높이를 과도하게 밀어내지 않는다.
- 긴 한국어 문장, 영어 플랫폼명, 숫자, URL 혼합 텍스트가 overflow를 만들지 않는다.

---

## 7. 긴 한국어 텍스트 안정성 기준

- `word-break`, `overflow-wrap`, `line-height`, `max-width`가 긴 정책 문장을 감당해야 한다.
- source excerpt는 기본 2-4줄 clamp 후 drawer/detail에서 full text를 보여준다.
- URL은 hostname 우선 표시, full URL은 copy/detail에서 확인 가능하게 한다.
- streaming-like update가 있다면 card height jump와 layout shift를 screenshot으로 확인한다.
- Pretext.js는 production dependency로 바로 추가하지 않고 CSS-only 전략과 비교하는 PoC 대상으로만 검토한다.

---

## 8. Badge / Source / Audit / Confidence / Provenance 기준

상태 badge:

- `Verified`: green
- `Limited evidence`: amber
- `No data found`: neutral/amber
- `Generation failed`: red or amber, source availability와 분리
- `Stale source`: amber
- `Unavailable source`: gray or red depending on severity

Source:

- source title, platform, URL/hostname, collected time, freshness, excerpt를 표시한다.
- source가 없는 답변은 confidence를 높게 보이게 하지 않는다.

Audit:

- admin/document UI에서는 ingestion status, collected time, failure reason을 추적 가능하게 표시한다.
- 일반 사용자 화면에는 내부 debug/raw JSON을 기본 노출하지 않는다.

Confidence:

- confidence는 badge와 짧은 설명을 함께 사용한다.
- confidence가 낮으면 후속 확인 또는 source review action을 안내한다.

Provenance:

- 정책 출처와 수집 시점을 answer와 연결한다.
- source provenance가 없으면 verified처럼 보이게 하지 않는다.

---

## 9. 제품 Agent 전달용 실행 프롬프트

```text
You are reviewing AdMate Compass UI.

Goal:
Audit the Compass RAG answer/source experience and produce UI review findings only.

Scope:
- question input
- answer summary
- source/evidence list
- source drawer/detail
- noDataFound, limited evidence, generation failed states
- mobile long text behavior
- accessibility/focus states

Hard constraints:
- Do not modify product code unless explicitly approved in a later implementation Gate.
- Do not modify RAG retrieval, crawling, ingestion, embeddings, reranking, model routing, prompt logic, confidence calculation, API contracts, DB schema, auth, environment, or secrets.
- Do not edit copied reference documents.
- Do not call external APIs.
- Do not use real advertiser names, campaign names, account information, raw campaign-level data, tokens, or credentials.

Design direction:
- Follow AdMate Operational Intelligence Console.
- Prioritize source trust, evidence readability, citation clarity, confidence state, Korean text stability, and calm operational UI.
- Separate noDataFound, limited evidence, and sources found but generation failed.
- Avoid cinematic dark, glassmorphism, decorative blobs, and marketing-style hero layouts.

Review output:
- screenshot audit notes
- P0/P1 UI issues
- source/evidence state matrix
- desktop and mobile text overflow notes
- no-touch areas confirmed
- recommended prompt/rules for a future implementation Gate
```
