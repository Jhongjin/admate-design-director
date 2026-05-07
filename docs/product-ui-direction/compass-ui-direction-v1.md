# Compass UI Direction v1

작성일: 2026-05-07
대상: AdMate Compass, Policy Intelligence Agent
목적: Compass가 AdMate 전체 운영 플랫폼 안에서 신뢰 가능한 정책/가이드 UI로 보이도록 제품별 디자인 방향을 정의한다.

---

## 1. Product Role

Compass는 광고 플랫폼 정책과 가이드를 근거 기반으로 답변하는 RAG 제품이다.

한 줄 기준:

```text
Compass는 정책을 답한다.
```

Compass UI는 "AI가 그럴듯하게 답했다"가 아니라 "어떤 source를 근거로 어느 정도 확신할 수 있는지"를 보여줘야 한다.

---

## 2. Design Objective

Compass의 디자인 목표:

- 답변, source, confidence, policy risk를 한 화면에서 읽기 쉽게 구성한다.
- 긴 한국어/영어 혼합 정책 문서가 깨지지 않게 한다.
- source quality와 citation 상태를 명확히 표시한다.
- AdMate 공통 운영 콘솔 톤을 유지한다.

추천 조합:

```text
Mintlify + Notion + Linear
```

AdMate 번역:

```text
문서형 readability
+ source/citation trust
+ workflow UI의 정밀한 상태 표현
```

---

## 3. Primary Surfaces

우선 정리할 화면:

| Surface | Direction |
|---|---|
| Question input | 질문 맥락, platform, policy domain을 명확히 받는다 |
| Answer card | 답변 요약, 근거 수준, 위험도, 후속 질문을 분리한다 |
| Source list | verified, partial, stale, unavailable 상태를 badge로 표시한다 |
| Source drawer | excerpt, URL, collected time, source quality를 보여준다 |
| Empty/error state | noDataFound와 generation failure를 분리한다 |
| Admin/document UI | 운영 콘솔 톤으로 정리하되 RAG 로직은 건드리지 않는다 |

---

## 4. Visual Direction

기본:

- background: `#F7F7F7`
- surface: `#FFFFFF`
- border: `#E5E5E5`
- text: `#0D0D0D`
- secondary: `#5E5E5E`

Accent:

- Compass accent: indigo-blue
- verified source: green
- limited evidence: amber
- policy risk critical: red

구성:

```text
Topbar
Question context row
Answer summary
Source evidence panel
Follow-up suggestions
History / recent policy questions
```

---

## 5. State Model

Compass는 아래 상태를 UI에서 분리해야 한다.

| State | UI Treatment |
|---|---|
| Ready | 질문 입력과 최근 source freshness 표시 |
| Searching | skeleton + source search progress |
| Answered with verified sources | answer card + verified source badge |
| Limited evidence | amber badge + "근거 제한" 설명 |
| noDataFound | 답변 생성보다 검색 실패/범위 밖임을 설명 |
| Sources found but generation failed | source는 보여주고 답변 생성 실패를 분리 |
| Policy risk | low/medium/high badge와 근거 연결 |

---

## 6. Taste Skill Translation

Compass에는 Taste Skill을 직접 설치하지 않는다.

허용 방향:

- redesign-skill reference
- minimalist-skill reference
- soft-skill reference

제한:

- RAG answer/source UI에만 적용한다.
- source readability와 Korean text stability를 우선한다.
- marketing hero, gradient-heavy, cinematic dark UI는 금지한다.

제품 Agent 전달 prompt seed:

```text
Use Taste Skill only as a design quality reference.
Follow AdMate Operational Intelligence Console.
Redesign only the Compass RAG answer/source UI.
Do not modify retrieval, API contract, DB, auth, environment, embeddings, reranking, or prompt logic.
Prioritize source trust, citation readability, confidence state, Korean text stability, and calm operational UI.
Avoid cinematic dark, glassmorphism, decorative blobs, and marketing-style hero layout.
```

---

## 7. Do Not Change

- RAG retrieval logic
- crawler/ingestion logic
- embedding/reranker/model routing
- API contracts
- DB schema
- auth
- environment variables
- source truthfulness or confidence calculation

Design Gate에서 바꿀 수 있는 것:

- layout
- spacing
- typography
- source card hierarchy
- badge/state treatment
- empty/error copy
- accessibility/focus state
