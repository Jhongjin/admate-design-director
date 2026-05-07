# Design Director Agent

Design Director Agent는 AdMate 전체 제품군의 디자인 기준을 중앙에서 정리하는 문서 repo다.

이 repo의 목표는 Compass, Sentinel, Lens, Foresight, Agent Core, Intelligence Library, Homepage, Creative Studio가 서로 다른 도구처럼 보이지 않고 하나의 통합 운영 플랫폼처럼 보이도록 디자인 원칙, UI review 기준, 제품별 prompt/rules를 정리하는 것이다.

---

## 1. 핵심 방향

AdMate의 UI 기준은 다음으로 통일한다.

```text
AdMate Operational Intelligence Console
= 광고 운영자가 매일 쓰는 신뢰형 업무 콘솔
+ 검증된 근거, 상태, 승인, audit 중심 정보 구조
+ 제품별 muted accent
+ 과장되지 않은 AI Agent 플랫폼 감각
```

AdMate는 화려한 AI SaaS 랜딩 페이지보다 실무형 운영 콘솔에 가깝다. 제품별 화면은 다를 수 있지만 배경, 표면, border, typography, density, 상태 표현 방식은 하나의 시스템처럼 이어져야 한다.

---

## 2. 문서 구조

| 경로 | 역할 |
|---|---|
| `AGENTS.md` | Design Director Agent 운영 지침 |
| `README.md` | repo 개요와 문서 지도 |
| `docs/design-system/admate-design-system-v1.md` | AdMate 공통 디자인 시스템 |
| `docs/design-system/admate-ui-review-checklist-v1.md` | 디자인 Gate와 UI QA checklist |
| `docs/product-ui-direction/*` | 제품별 UI 방향 |
| `docs/references/*` | 보조 reference log와 PoC notes |
| `docs/reference/admate-docs/*` | copied AdMate reference 원문 |

주의:

- `docs/reference/admate-docs/`는 원본 reference 보관 영역이다.
- 새 Design Director 문서는 `docs/design-system/`, `docs/product-ui-direction/`, `docs/references/`에 둔다.
- copied reference 문서는 수정하지 않는다.

---

## 3. 제품별 문서

| 제품/영역 | 문서 |
|---|---|
| Compass | `docs/product-ui-direction/compass-ui-direction-v1.md` |
| Lens | `docs/product-ui-direction/lens-ui-direction-v1.md` |
| Foresight | `docs/product-ui-direction/foresight-ui-direction-v1.md` |
| Sentinel / Openclaw | `docs/product-ui-direction/sentinel-openclaw-ui-direction-v1.md` |
| Intelligence Library | `docs/product-ui-direction/intelligence-library-ui-direction-v1.md` |
| Neuform reference | `docs/references/neuform-reference-log-v1.md` |
| Pretext.js PoC | `docs/references/pretext-ui-poc-notes-v1.md` |

---

## 4. Taste Skill 정책

Taste Skill은 이 repo에서 바로 설치하거나 제품 repo에 적용하지 않는다.

현재 정책:

```text
Taste Skill
→ Design Director reference
→ AdMate 전용 prompt/rules로 번역
→ 제품 Agent에게 제한적으로 전달
→ screenshot QA 후 채택 여부 기록
```

우선 PoC 후보:

- Creative Studio
- Homepage

제한적 적용 후보:

- Compass
- Foresight
- Intelligence Library

직접 적용 금지:

- Sentinel critical operations
- Lens capture output 자체
- 제품 로직, RAG, DB, auth, capture output

---

## 5. 안전 원칙

이 repo는 문서 작업만 수행한다.

금지:

- 제품 repo 수정
- 코드/API/DB/env 변경
- copied reference 원문 수정
- 실제 secret 값 출력 또는 커밋
- 실제 캠페인 raw data를 prompt나 예시에 포함

reference 문서에 금지 키워드명이 들어 있을 수 있다. 이는 실제 secret 값이 아니라 "출력/커밋 금지 목록" 문맥이면 reference-only hit로 분리 보고한다.

---

## 6. 검증

문서 변경 후 기본 검증:

```powershell
git diff --check
git status --short
```

추가로 수행할 검증:

- secret pattern scan
- large file scan
- reference keyword hit와 실제 secret value 구분
