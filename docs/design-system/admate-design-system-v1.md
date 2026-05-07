# AdMate Design System v1

작성일: 2026-05-07
문서 상태: 초안 v1
목적: AdMate 전체 제품군이 하나의 통합 운영 플랫폼처럼 보이도록 공통 UI 기준을 정의한다.

---

## 1. 디자인 정의

AdMate의 디자인 기준은 `AdMate Operational Intelligence Console`이다.

```text
AdMate Operational Intelligence Console
= 차분한 운영 콘솔
+ 검증된 근거/상태/로그 중심 UI
+ 제품별 muted accent
+ 내부 지식과 Agent 실행 흐름이 보이는 정보 구조
```

AdMate는 개별 AI 도구 모음이 아니라 광고 운영 생애주기를 연결하는 Agent 플랫폼이다. 따라서 화면은 "멋진 AI 느낌"보다 운영자가 매일 보고 판단할 수 있는 밀도, 안정감, 추적성을 우선한다.

---

## 2. 원칙

### 2.1 Functional Calm

UI는 차분해야 한다. 색상, motion, 장식은 업무 판단을 돕는 곳에서만 쓴다.

### 2.2 Evidence First

Compass의 source, Sentinel의 alert reason, Foresight의 benchmark basis, Intelligence Library의 provenance처럼 근거 정보가 화면의 중심이어야 한다.

### 2.3 Traceable Action

중요 action은 누가, 언제, 왜 실행했는지 확인할 수 있어야 한다. audit, operator history, approval state를 숨기지 않는다.

### 2.4 Product Accent, Shared System

제품별 accent는 허용하지만 전체 구조는 공유한다. 배경, surface, border, radius, typography, 버튼, badge 규칙은 같아야 한다.

### 2.5 No Logic Drift

디자인 Gate는 제품 로직을 바꾸지 않는다. RAG, DB, auth, API, capture output, threshold, model logic은 디자인 작업 범위 밖이다.

---

## 3. Core Tokens

### 3.1 Color

| Token | Value | Use |
|---|---|---|
| App background | `#F7F7F7` | 전체 앱 배경 |
| Surface | `#FFFFFF` | card, panel, table |
| Hover | `#F4F4F4` | row/button hover |
| Active | `#ECECEC` | selected state |
| Border | `#E5E5E5` | cards, tables, controls |
| Primary text | `#0D0D0D` | 제목, 핵심 텍스트 |
| Secondary text | `#5E5E5E` | 보조 설명 |
| Muted text | `#9A9A9A` | helper, timestamp |
| Info accent | `#5E6AD2` | info, policy, selected |
| Info surface | `#ECEDF9` | info badge background |
| Success | `#177D4E` | normal, approved |
| Success surface | `#EFFAF4` | success badge background |
| Warning | `#9E5700` | review, stale, warning |
| Warning surface | `#FFF8EC` | warning badge background |
| Critical | `#D93025` | blocked, critical |
| Critical surface | `#FEF2F1` | critical badge background |

금지:

- 강한 purple/blue gradient 중심 테마
- cinematic dark UI를 업무 화면 기본으로 사용
- glassmorphism 과다 사용
- decorative blob/orb/bokeh background

### 3.2 Product Accent

| Product | Accent | Use |
|---|---|---|
| Agent Core / Openclaw | graphite / black | core, automation, governance |
| Compass | indigo-blue | policy, RAG, source |
| Sentinel | emerald | monitoring, validation, normal |
| Lens | violet | capture, proof, visual QA |
| Foresight | amber / analytic blue | forecast, benchmark, analysis |
| Intelligence Library | slate-indigo | knowledge, approval, wiki |
| Creative Studio | controlled visual accent | storyboard, presentation concept |

Accent는 navigation, selected tab, product label, icon tint, chart highlight에 제한적으로 사용한다.

---

## 4. Typography

기본 font stack:

```css
Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif
```

권장 크기:

| Use | Size |
|---|---|
| Topbar title | 13px |
| Sidebar label | 12-13px |
| Section label | 11px |
| Body | 13-14px |
| Helper | 11-12px |
| KPI number | 22-28px |
| Compact table | 12-13px |

원칙:

- viewport width로 font-size를 스케일하지 않는다.
- letter-spacing은 기본 0으로 둔다.
- 한국어 긴 문장은 버튼/카드 안에서 넘치지 않게 줄바꿈을 허용한다.
- 업무 UI 내부에서 hero-scale typography를 남용하지 않는다.

---

## 5. Layout

기본 구조:

```text
Sidebar
Topbar
Summary / KPI
Primary workspace
Detail drawer or detail page
Audit / history / source panel
```

권장:

- Sidebar width: 220px
- Topbar height: 44px
- Content max-width: 1100-1280px
- Card radius: 8px 이하 기본
- Shadow보다 border와 spacing으로 계층 구성
- Page section을 floating card처럼 과하게 감싸지 않는다.

업무 화면은 첫 viewport에서 다음을 보여줘야 한다.

- 현재 상태
- 다음으로 해야 할 action
- 근거 또는 최근 이벤트
- 위험/주의 여부

---

## 6. Components

### 6.1 Button

원칙:

- 짧은 한국어 동사형을 사용한다.
- primary action은 한 화면에 과하게 늘리지 않는다.
- destructive/critical action은 확인 단계를 둔다.
- icon이 적합하면 icon+tooltip을 우선한다.

형태:

- height: 28-34px
- border-radius: 4-6px
- padding: 6px 12px
- font-size: 12-13px

### 6.2 Badge

상태는 badge로 통일한다.

| State | Tone |
|---|---|
| 정상 / 승인 | success |
| 검토 / 대기 | info 또는 warning |
| 주의 / stale | warning |
| 위험 / 차단 | critical |
| 폐기 / archived | gray |

### 6.3 Card

Card는 반복 item, summary, detail grouping에 사용한다.

금지:

- card 안에 또 다른 decorative card를 중첩
- 마케팅형 oversized card grid를 업무 화면 기본 구조로 사용
- source/audit보다 일러스트를 더 크게 배치

### 6.4 Table / List

AdMate 업무 화면은 table/list density가 중요하다.

원칙:

- header는 명확히 고정한다.
- row hover는 subtle하게 둔다.
- 상태는 text color만으로 구분하지 않고 badge를 쓴다.
- raw JSON/debug 정보는 기본 노출하지 않는다.
- 긴 한국어 reason은 line-clamp 또는 drawer 상세로 보낸다.

### 6.5 Drawer / Detail

상세 정보는 drawer 또는 detail page로 분리한다.

적합한 정보:

- source excerpt
- alert reason
- operator action history
- benchmark basis
- approval history
- capture metadata

---

## 7. Product-Specific Guidance

### Compass

- Answer보다 source 신뢰도와 citation이 먼저 보이게 한다.
- noDataFound, limited evidence, generation failed but sources found 상태를 분리한다.
- 긴 한국어/영어 혼합 policy text overflow를 검증한다.

### Sentinel / Openclaw

- critical operation은 motion과 decoration을 최소화한다.
- alert severity, suppression, audit history, operator action이 명확해야 한다.
- threshold나 monitoring logic을 디자인 작업에서 바꾸지 않는다.

### Lens

- capture output 자체는 디자인 시스템 적용 대상이 아니다.
- 운영자 UI, 요청 목록, 품질 상태, 결과 관리 화면에만 적용한다.
- output fidelity가 UI polish보다 우선이다.

### Foresight

- spreadsheet와 analytics console 사이의 밀도를 유지한다.
- benchmark basis, confidence range, recent data 기준을 명확히 보여준다.
- raw campaign-level data를 화면 예시나 LLM prompt에 넣지 않는다.

### Intelligence Library

- candidate/draft/approved/deprecated 상태를 강하게 구분한다.
- source/provenance와 approval history가 핵심이다.
- 사람용 Wiki와 Agent용 approved knowledge layer를 혼동하지 않는다.

### Homepage / Creative Studio

- Homepage와 Creative Studio는 image-first workflow PoC 후보가 될 수 있다.
- 단, AdMate의 운영 신뢰감을 해치는 gradient-heavy landing style은 피한다.
- Creative Studio 외 영역에서 animation은 신중하게 제한한다.

---

## 8. Motion and Images

업무 UI의 motion 원칙:

- 상태 변화 이해를 돕는 micro interaction만 사용한다.
- Sentinel critical alert, approval, fail-closed 화면에는 motion을 최소화한다.
- data table, source list, audit log는 안정적인 rendering을 우선한다.

Images workflow:

- Creative Studio 중심으로 검토한다.
- Homepage는 hero/section visual exploration 후보로만 사용한다.
- Compass/Foresight/Intelligence Library는 screenshot QA와 layout reference 수준으로 제한한다.
- Lens capture output에는 직접 적용하지 않는다.

---

## 9. Prompt Translation Rule

제품 Agent에게 디자인 작업을 넘길 때는 아래 형식을 따른다.

```text
Use AdMate Operational Intelligence Console.
Do not modify product logic, API contracts, DB, auth, RAG, environment, or capture output.
Apply only layout, visual hierarchy, spacing, typography, component states, and accessibility improvements.
Prioritize status, evidence, approval, audit, and Korean text stability.
Avoid cinematic dark, decorative blobs, glassmorphism, and gradient-heavy AI SaaS styling.
```

제품별 추가 금지 영역은 각 UI direction 문서를 따른다.
