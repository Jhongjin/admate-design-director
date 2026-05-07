# Neuform Reference Log v1

작성일: 2026-05-07
문서 상태: 초안 v1
목적: Neuform과 유사한 외부 디자인 reference를 AdMate에 그대로 복사하지 않고, 제품별 prompt/rules로 번역하기 위한 기록 형식을 정의한다.

---

## 1. Position

Neuform은 AdMate 디자인 시스템의 최종 기준이 아니다.

AdMate에서 Neuform은 다음 용도로만 사용한다.

- 빠른 visual exploration
- layout/density/component treatment 참고
- 제품별 prompt snippet 후보 기록
- Design Director가 제품 Agent에게 전달할 rules로 번역

최종 기준은 항상 다음이다.

```text
AdMate Operational Intelligence Console
```

---

## 2. Adoption Rule

Neuform reference를 채택할 때는 template을 복사하지 않는다.

추출 가능한 것:

- layout 구조
- typography rhythm
- status card density
- source/review/approval panel 구성
- empty state 처리
- onboarding 또는 candidate submit flow

그대로 가져오면 안 되는 것:

- brand color
- gradient/glass style
- decorative visual
- marketing-first hero layout
- AdMate 업무 정보와 무관한 card-heavy composition

---

## 3. Reference Entry Template

Neuform 또는 외부 visual reference를 발견하면 아래 형식으로 기록한다.

```text
Reference:
- source:
- url:
- template_or_skill_name:
- captured_at:
- visual_traits:
- useful_for:
- avoid:
- AdMate_translation:
- target_product:
- target_surface:
- adoption_gate:
- status:
- notes:
```

Status 값:

```text
Candidate
Useful Pattern
Translated Prompt
Rejected
Archived
```

---

## 4. Selection Criteria

선정 기준:

- 운영자가 매일 써도 피로하지 않은가?
- status, source, audit, approval, evidence를 가리지 않는가?
- 한국어 텍스트 길이를 감당할 수 있는가?
- table/list density가 업무에 맞는가?
- 모바일에서 버튼/라벨이 넘치지 않는가?
- 강한 gradient, glass, cinematic dark, decorative blob에 의존하지 않는가?
- 제품별 금지 영역을 침범하지 않는가?

---

## 5. Product Reference Targets

| Product | Neuform에서 찾을 유형 | 적용 후보 |
|---|---|---|
| Homepage | clean AI platform landing, ecosystem cards | hero, ecosystem, final CTA |
| Command Center | executive dashboard, project cards | Engine hero card, product progress cards |
| Compass | AI answer, source cards, documentation UI | RAG answer/source/noDataFound |
| Sentinel | monitoring console, incident timeline | alert, validation, operator action |
| Lens | quality dashboard, image review grid | capture jobs, golden sample QA |
| Foresight | analytics dashboard, table upload flow | benchmark, forecast, report profile |
| Intelligence Library | wiki, review queue, submission form | candidate submit, knowledge detail |
| Creative Studio | video/storyboard/presenter landing | script, storyboard, presenter setup |

---

## 6. Initial Reference Notes

### Neuform Community Featured

Reference:

- source: Neuform
- url: `https://neuform.ai/community/featured`
- template_or_skill_name: community featured gallery
- captured_at: reference 문서 기준
- visual_traits: reusable templates, design system gallery, DESIGN.md source 탐색
- useful_for: Homepage, Intelligence Library, Compass source cards, Foresight benchmark workspace, Creative Studio storyboard
- avoid: template-driven look, over-decorated landing composition
- AdMate_translation: 구조/밀도/타이포/상태 표현만 추출
- target_product: multiple
- target_surface: non-critical UI
- adoption_gate: Design Reference Review
- status: Candidate
- notes: 최종 기준은 Openclaw 운영 콘솔 톤으로 유지한다.

### Neuform Skills

Reference:

- source: Neuform
- url: `https://neuform.ai/skills`
- template_or_skill_name: skills gallery
- captured_at: reference 문서 기준
- visual_traits: layout, typography, motion, color, shadow, component treatment를 prompt skill 단위로 정리
- useful_for: 제품별 UI prompt library
- avoid: style variance가 제품 간 통일성을 해치는 경우
- AdMate_translation: product-specific design prompt/rules로 변환
- target_product: Compass, Foresight, Intelligence Library, Creative Studio
- target_surface: prompt/rules and safe PoC
- adoption_gate: Prompt Translation
- status: Candidate
- notes: Design Director가 중간 interpretation layer 역할을 한다.

---

## 7. Translation Example

Neuform reference를 제품 Agent에게 넘길 때는 아래처럼 번역한다.

```text
Reference type: compact source card layout.
AdMate translation: Use the density and metadata placement only.
Product: Compass.
Surface: RAG source list and source drawer.
Constraints: Do not modify RAG retrieval, API, DB, auth, environment, or prompt logic.
Visual constraints: AdMate neutral background, border-first cards, indigo accent only for source state.
QA: Korean text overflow, mobile drawer, noDataFound state, source verified/limited/stale badge.
```

---

## 8. Review Cadence

Neuform reference는 발견 즉시 채택하지 않는다.

권장 흐름:

```text
Reference 발견
→ log 작성
→ AdMate translation 작성
→ product boundary 확인
→ safe PoC 후보 선정
→ screenshot QA
→ product Agent prompt에 반영
```
