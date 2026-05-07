# AdMate Platform Theme Recommendations v1

작성일: 2026-05-06
목적: AdMate 전체 제품군의 UI/UX 테마가 서로 분리되어 보이지 않도록, 외부 디자인 레퍼런스를 검토하고 제품별 추천 테마와 적용 원칙을 정리한다.

## 1. 최상위 판단

AdMate는 화려한 AI SaaS 랜딩 페이지보다, 광고 운영자가 매일 쓰는 신뢰형 업무 콘솔에 가까워야 한다.

핵심 방향:

```text
AdMate Operational Intelligence Console
= 차분한 운영 콘솔
+ 검증된 근거/상태/로그 중심 UI
+ 제품별 muted accent
+ 내부 지식과 Agent 실행 흐름이 보이는 정보 구조
```

기준 문서:

- `design/openclaw-theme-reference.md`

## 2. 외부 레퍼런스 판단

### getdesign.md

판단: 적극 참고.

이유:

- DESIGN.md 형태로 AI coding agent가 UI 기준을 재사용하기 좋다.
- Vercel, Stripe, Figma, Notion, Supabase, Linear, Claude, Sentry, IBM, Airtable 등 AdMate와 맞는 레퍼런스가 많다.
- 2026-05-06 기준 70개 DESIGN.md 파일이 등록되어 있다.

AdMate 적용:

- Linear: 정밀한 업무 UI, issue/status 중심 카드
- IBM Carbon: enterprise data console 구조
- Supabase/Mintlify: 기술 문서와 신뢰형 developer/product UI
- Notion: 읽기 좋은 knowledge/wiki UI
- Sentry: issue, alert, trace, event log 패턴
- Airtable: Foresight/benchmark table UI 참고

출처:

- https://getdesign.md/

### Neuform

판단: 적극 참고로 상향.

이유:

- 커뮤니티 템플릿과 reusable design system gallery 성격이다.
- featured gallery는 public template, reusable design system, `DESIGN.md` source를 함께 탐색하는 구조라 Agent 작업 지시로 옮기기 좋다.
- skills 페이지는 layout system, typography, motion, color, shadow, component treatment 같은 재사용 가능한 디자인 move를 prompt skill 단위로 정리하는 구조라 AdMate 제품별 UI prompt library로 전환하기 좋다.
- 단, AdMate 전체 운영 콘솔 기준으로는 과하게 template-driven이 되지 않도록 reference만 취하고, 최종 기준은 Openclaw 운영 콘솔 톤으로 유지한다.

AdMate 적용:

- landing/feature section의 카드 구성 참고
- 제품별 빈 상태, 안내 섹션, lightweight prototype 참고
- 제품별 `DESIGN.md`/prompt snippet 후보를 저장할 reference source로 사용
- Intelligence Library/Wiki 후보 제출, Compass answer/source card, Foresight benchmark upload, Creative Studio landing storyboard의 빠른 visual exploration에 사용
- Neuform template을 그대로 복사하지 않고, "구조/밀도/타이포/상태 표현"만 추출한다.

출처:

- https://neuform.ai/community/featured
- https://neuform.ai/skills

### Variant

판단: 보류.

이유:

- community 페이지가 sign in/sign up 뒤에 있어 현재 공개적으로 충분히 검토하기 어렵다.
- 추후 로그인 기반 visual reference 탐색 시 보조 자료로만 사용한다.

출처:

- https://variant.com/community

### Aura

판단: 컴포넌트/랜딩 참고용, 핵심 기준으로는 보류.

이유:

- AI landing page builder와 HTML/Figma export 중심이다.
- SaaS, portfolio, e-commerce, service, mobile animation 등 broad template gallery 성격이 강하다.
- AdMate의 내부 운영 콘솔 기준으로는 과하게 마케팅/템플릿 느낌이 날 수 있다.

AdMate 적용:

- 홈페이지 CTA, lightweight section, empty state, onboarding card 참고
- Intelligence Library/Wiki의 후보 제출 화면 같은 작은 UI 패턴 참고
- 단, Sentinel/Engine/Compass의 운영 화면에는 직접 테마로 쓰지 않는다.

출처:

- https://www.aura.build/browse/components

## 3. 채택할 디자인 조합

AdMate 전체 기준은 아래 조합을 채택한다.

```text
IBM Carbon의 enterprise 구조
+ Linear의 정밀한 workflow UI
+ Supabase/Mintlify의 기술 신뢰감
+ Notion의 읽기 좋은 knowledge UI
+ Sentry의 issue/status/audit 패턴
```

피할 것:

- 강한 purple/blue gradient 위주의 AI SaaS 테마
- glassmorphism 과다 사용
- cinematic dark UI
- marketing hero 중심 레이아웃
- 소비자 앱처럼 둥글고 장식적인 카드
- source/audit/log보다 일러스트가 앞서는 구성

## 4. 공통 UI 원칙

색상:

- base background: `#F7F7F7`
- surface/card: `#FFFFFF`
- border: `#E5E5E5`
- primary text: `#0D0D0D`
- secondary text: `#5E5E5E`
- muted text: `#9A9A9A`

형태:

- 카드 radius는 8px 이하를 기본으로 한다.
- 그림자보다 border와 spacing으로 계층을 만든다.
- 상태는 badge로 표현한다.
- 버튼은 짧은 한국어 동사형을 사용한다.
- 개발자 용어는 사용자 문구로 바꾼다.
- raw JSON/debug 정보는 기본 노출하지 않는다.

정보 구조:

- Topbar
- 요약/KPI
- 주요 작업 또는 source list
- 상세 drawer 또는 detail page
- audit/operator history

## 5. 제품별 추천 테마

### AdMate Engine / Openclaw / Sentinel Live Monitoring

추천 조합:

```text
IBM Carbon + Linear + Sentry
```

이유:

- 권한, audit, operator action, alert, status, workflow를 가장 많이 다룬다.
- 감성보다 상태 판단과 추적성이 중요하다.

적용:

- dense dashboard
- status badge
- audit timeline
- operator action log
- alert severity card
- fail-closed/config/security warning UI

Accent:

- graphite / black
- emerald for normal
- amber for review
- red for critical

### AdMate Compass

추천 조합:

```text
Mintlify + Notion + Linear
```

이유:

- 정책/가이드/RAG/source/citation을 읽기 좋게 보여줘야 한다.
- 사용자는 답변보다 근거 신뢰도를 먼저 확인해야 한다.

적용:

- answer card
- verified source card
- citation/source quality badge
- noDataFound state
- generation failed but sources found state
- document/admin UI는 후순위로 운영 콘솔 톤에 맞춘다.

Accent:

- indigo-blue
- source verified green
- limited evidence amber

### AdMate Lens

추천 조합:

```text
Linear + Sentry + minimal Apple-like spacing
```

이유:

- 캡처 결과물 자체는 픽셀 매칭이 중요하므로 디자인 변경 대상이 아니다.
- 운영자 UI, 작업 목록, 검수 상태, golden sample quality harness를 정리해야 한다.

적용:

- capture request list
- quality status badge
- metadata detail modal
- golden sample manifest status
- diff/failure report UI

주의:

- `src/lib/capture`, capture engine, injection, composite 결과물은 임의 변경 금지.

Accent:

- violet
- quality ok green
- review amber
- failure red

### AdMate Foresight

추천 조합:

```text
Airtable + IBM Carbon + Linear
```

이유:

- benchmark, prediction, analysis, report table, segment comparison이 중심이다.
- spreadsheet와 analytics console 사이의 UI가 적합하다.

적용:

- benchmark table
- metric cards
- forecast panel
- confidence/effect size display
- upload mapping UI
- report personalization template profile UI

Accent:

- amber
- analytic blue
- confidence green/amber/red

### AdMate Homepage / Command Center

추천 조합:

```text
Vercel + Linear + Notion
```

이유:

- 브랜드 전달력과 운영 현황의 신뢰감을 같이 보여줘야 한다.
- Command Center는 read-only executive dashboard이므로 과한 장식보다 구조적 명확성이 중요하다.

적용:

- clean hero
- ecosystem cards
- Engine horizontal core card
- product progress cards
- public CTA: `AdMate 시작하기`
- 신청 페이지명: `AdMate 이용 신청`

Accent:

- neutral black/white
- product accent만 제한적으로 사용

### AdMate Intelligence Library / Wiki

추천 조합:

```text
Notion + Mintlify + Linear + Sentry
```

이유:

- 사람이 읽는 Wiki와 Hermes/Agent가 참조하는 approved knowledge layer를 함께 다룬다.
- 지식 상태, 승인, stale, pinned, source 연결을 명확히 보여줘야 한다.

적용:

- knowledge list/detail
- review queue
- approval history
- source link graph/list
- skill candidate queue
- pinned core knowledge badge
- stale/deprecated state

Accent:

- slate/indigo
- approved green
- review blue
- stale amber
- deprecated gray

## 6. 제품별 액센트 컬러 후보

| Product | Accent | Use |
|---|---|---|
| Engine | graphite / black | core, automation, governance |
| Compass | indigo-blue | policy, RAG, source |
| Sentinel | emerald | monitoring, validation, normal state |
| Lens | violet | capture, proof, visual QA |
| Foresight | amber | forecast, benchmark, analysis |
| Intelligence Library | slate-indigo | knowledge, approval, wiki |

Status colors:

| Status | Color |
|---|---|
| approved / normal | green |
| review / pending | blue or amber |
| warning | amber |
| critical / blocked | red |
| deprecated / archived | gray |

## 7. 적용 우선순위

1. Compass 사용자-facing RAG UI 정리
2. Openclaw/Sentinel access request와 governance UI 정리
3. Intelligence Library / Wiki admin UI 설계
4. Foresight benchmark/analysis UI 정리
5. Lens golden sample quality UI 정리
6. Homepage는 이미 맞춰진 구조를 유지하며 CTA/Command Center만 보강

## 8. Agent 작업 지시 원칙

디자인 작업을 지시할 때는 다음을 명시한다.

```text
AdMate Operational Intelligence Console 기준을 따른다.
기능/API/DB/RAG/권한 로직은 변경하지 않는다.
Openclaw theme token과 이 문서의 제품별 테마를 우선 적용한다.
화려한 gradient/glass/cinematic dark UI는 피한다.
상태, 근거, 승인, audit, source를 명확히 보이게 한다.
```

## 9. 다음 작업 후보

- AdMate Design System v1 문서화
- Compass UI-3 page layout refresh
- Intelligence Library / Wiki UI IA visual 기준 추가
- Foresight benchmark table/report UI theme plan
- Lens golden sample quality dashboard theme plan

## 10. Neuform 기반 테마 선정 기록 방식

Neuform에서 마음에 드는 template/skill을 발견하면 아래 형식으로 중앙 문서나 제품별 UI plan에 기록한다.

```text
Reference:
- source: Neuform
- url:
- template_or_skill_name:
- visual_traits:
- useful_for:
- avoid:
- AdMate_translation:
- target_product:
- target_surface:
```

선정 기준:

- 운영자가 매일 써도 피로하지 않은가
- status, source, audit, approval, evidence 같은 AdMate 핵심 정보를 가리지 않는가
- 카드/섹션/폼/목록 구조가 실제 제품 화면에 이식 가능한가
- 한국어 텍스트 길이를 감당할 수 있는가
- 모바일에서 버튼/라벨이 넘치지 않는가
- 강한 gradient, glass, cinematic dark, decorative blob에 의존하지 않는가

제품별로 우선 저장할 Neuform reference 유형:

| Product | Neuform에서 찾을 유형 | 적용 후보 |
|---|---|---|
| Homepage | clean AI platform landing, ecosystem cards | Hero, ecosystem, final CTA |
| Command Center | executive dashboard, project cards | Engine hero card, product progress cards |
| Compass | AI answer, source cards, documentation UI | RAG answer/source/noDataFound |
| Sentinel | monitoring console, incident timeline | alert, validation, operator action |
| Lens | quality dashboard, image review grid | capture jobs, golden sample QA |
| Foresight | analytics dashboard, table upload flow | benchmark, forecast, report profile |
| Intelligence Library | wiki, review queue, submission form | candidate submit, knowledge detail |
| Creative Studio | video/storyboard/presenter landing | script/storyboard/presenter setup |

## 11. Pretext.js 검토

판단: 지금 당장 core dependency로 전면 도입하지 말고, 텍스트가 많은 화면의 실험 후보로 보류한다.

Pretext는 JavaScript/TypeScript 기반 텍스트 측정/레이아웃 라이브러리다. DOM measurement 없이 paragraph height, line count, line ranges를 계산해 layout reflow를 줄이는 것이 핵심이다. 한국어/다국어 혼합 텍스트, AI 응답, 채팅 버블, 가변 높이 리스트, rich inline chip/badge 같은 화면에서 장점이 있다.

출처:

- https://pretextjs.dev/ko/pretext-demo
- https://github.com/chenglou/pretext

AdMate에 녹일 수 있는 후보:

| Surface | 적용 아이디어 | 우선순위 |
|---|---|---|
| Compass RAG answer/source | 긴 답변과 source excerpt 높이 예측, tight answer bubble, source card layout 안정화 | 중 |
| Intelligence Library/Wiki | knowledge list/detail의 긴 한국어 summary, tags/chips와 본문 inline layout | 중 |
| Sentinel alert log | 긴 alert reason/operator note가 포함된 virtualized log list | 낮음-중 |
| Foresight report comments | 개인화 보고서 코멘트 preview, 긴 문장 높이 예측 | 중 |
| Creative Studio | 홍보 페이지/영상 storyboard의 인터랙티브 타이포그래피 demo | 낮음, 실험용 |
| Homepage | hero headline 주변 creative text flow | 낮음, 과하면 운영 신뢰감 저하 |

권장 PoC:

```text
Gate UI-Pretext-1
= Compass 또는 Intelligence Library의 긴 한국어 텍스트 card에서
  layout shift 감소와 모바일 text overflow 방지 효과만 검증한다.
```

PoC 범위:

- production dependency 도입 전 별도 branch 또는 sandbox에서 검토
- Canvas/Intl.Segmenter browser support 확인
- Korean `wordBreak: keep-all`과 AdMate font stack에서 정확도 확인
- 기존 CSS line-clamp/overflow 전략과 비교
- visual decoration보다 "텍스트 안정성"에 집중

바로 적용하지 않는 이유:

- AdMate의 핵심 UI는 안정성과 신뢰성이 우선이라 creative typography는 과하면 방해가 된다.
- text measurement library는 font/letter-spacing/line-height sync가 맞지 않으면 오히려 불일치가 생길 수 있다.
- 현재 제품군은 기능 안정화와 권한/데이터 흐름이 더 중요하다.

사용하면 좋은 곳:

- 많은 긴 텍스트를 빠르게 렌더링해야 하는 화면
- 한국어/영어/숫자/칩이 섞인 source/knowledge card
- AI streaming 답변에서 bubble height/layout shift가 거슬리는 경우

피해야 할 곳:

- Lens capture output 자체
- Command Center executive dashboard의 핵심 KPI 영역
- Sentinel critical alert 화면의 필수 정보 영역
- 과한 text animation이 업무 판단을 방해하는 화면
