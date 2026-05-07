# Taste Skill / AI Design Workflow Notes v1

작성일: 2026-05-07

목적: Design Director Agent가 AI 기반 UI/랜딩/프로덕트 화면 개선 작업을 할 때 참고할 `taste-skill`과 관련 AI design workflow를 정리한다. 이 문서는 AdMate 전체 디자인 시스템의 보조 레퍼런스이며, 제품 repo 코드에 직접 적용하기 전 반드시 별도 Gate로 검토한다.

## 1. Reference

### YouTube

- Title: `Redesigning Websites with GPT-5.5 & Images 2.0`
- URL: https://youtu.be/PFO01z7Qe38
- 공개 요약 기준 workflow:
  - Codex/GPT 기반 landing page 생성
  - Claude vs GPT/Codex output 비교
  - Taste Skill로 redesign
  - Images 2.0으로 section visual 생성/교체
  - Seedance 2.0으로 static image animation
  - final layout refinement

참고 요약 출처:

- https://www.thinkingabout.ai/insights/uncategorized/redesigning-websites-with-gpt-5-5-and-images-2-0/
- https://videohighlight.com/v/PFO01z7Qe38

### Taste Skill

- GitHub: https://github.com/Leonxlnx/taste-skill
- Repo 설명 요약: AI가 만든 UI가 generic/slop처럼 보이지 않도록 layout, typography, motion, spacing, image reference workflow를 강화하는 portable Agent Skill 모음.
- 설치 예시:

```bash
npx skills add https://github.com/Leonxlnx/taste-skill
```

## 2. Taste Skill에서 참고할 개념

Taste Skill은 단일 visual style을 강제하기보다, UI output의 평균 품질을 높이기 위한 design instruction package에 가깝다.

주요 skill 유형:

| Skill | 용도 | AdMate 적용 판단 |
|---|---|---|
| taste-skill | 범용 premium frontend output | Design Director 기본 참고 후보 |
| gpt-taste | Codex/GPT 계열에 더 강한 layout/motion enforcement | Homepage/Creative Studio 실험 후보 |
| image-to-code-skill | image reference 생성 후 code 구현 | Creative Studio, Homepage hero, Compass source UI PoC 후보 |
| redesign-skill | 기존 UI audit 후 redesign | Compass, Foresight, Sentinel, Lens 운영 UI 개선 후보 |
| soft-skill | calm, polished, whitespace 중심 | Intelligence Library/Wiki, Foresight report UI 후보 |
| minimalist-skill | Notion/Linear 계열 restrained UI | Compass, Command Center, Wiki 후보 |
| imagegen-frontend-web | web comp image 생성 | Creative Studio storyboard, Homepage visual exploration |
| imagegen-frontend-mobile | mobile screen/flow comp 생성 | Access request, Compass mobile source panel 후보 |
| brandkit | palette/type/identity board | AdMate Design System 보조 후보 |

Taste Skill README의 설정 개념:

| Dial | 의미 | AdMate 권장 |
|---|---|---|
| DESIGN_VARIANCE | layout 실험 강도 | 운영 콘솔은 낮음-중간, Creative는 중간-높음 |
| MOTION_INTENSITY | motion 깊이 | 업무 UI는 낮음, landing/Creative는 중간 |
| VISUAL_DENSITY | viewport당 정보량 | Command Center/Foresight는 중간-높음, Homepage는 중간 |

## 3. AdMate에 적용할 수 있는 Workflow

권장 workflow:

```text
1. 제품/화면의 업무 목적 정의
2. AdMate design system 기준 확인
3. Taste Skill 또는 Neuform reference로 방향 탐색
4. image reference 또는 redesign prompt 생성
5. Design Director가 화면별 UX risk 검토
6. 제품 Agent가 코드 구현
7. screenshot/production smoke로 Design QA
8. 반영 결과를 design note에 기록
```

중요: Taste Skill은 "최종 기준"이 아니라 "디자인 탐색/품질 향상 보조 도구"로만 사용한다. AdMate의 최종 기준은 `AdMate Operational Intelligence Console`이다.

## 4. 제품별 적용 후보

### Homepage / Command Center

사용 후보:

- taste-skill
- minimalist-skill
- imagegen-frontend-web

적용:

- hero/CTA visual exploration
- Command Center card hierarchy
- Engine hero card polish
- executive dashboard density 조정

주의:

- public read-only surface이므로 관리자 링크/입력 링크 노출 금지.
- cinematic dark/gradient-heavy marketing theme 금지.

### Compass

사용 후보:

- redesign-skill
- minimalist-skill
- soft-skill

적용:

- RAG answer/source card hierarchy
- noDataFound/generation-limited state
- mobile source drawer
- long Korean answer/source text layout

주의:

- RAG/API/DB/search logic은 디자인 작업에서 수정 금지.
- source/evidence 신뢰도가 visual decoration보다 우선.

### Sentinel / Openclaw

사용 후보:

- redesign-skill
- minimalist-skill

적용:

- access request approval UI
- alert timeline
- operator action/audit log
- fail-closed/security warning surface

주의:

- operational clarity 우선.
- 화려한 motion은 incident/alert UI에서 피한다.

### Lens

사용 후보:

- redesign-skill
- image-to-code-skill for operator UI only

적용:

- capture preview workspace
- quality status dashboard
- golden sample QA UI

주의:

- capture output itself, rendering, injection, composite fidelity는 visual design experiment 대상이 아니다.
- GDN icon fidelity 같은 output 정확도는 platform reference 우선.

### Foresight

사용 후보:

- soft-skill
- minimalist-skill
- imagegen-frontend-web for report concept

적용:

- benchmark table
- forecast panel
- report personalization profile
- upload mapping flow

주의:

- data table/readability가 motion/decoration보다 우선.
- raw campaign-level data 노출 금지.

### Intelligence Library / Wiki

사용 후보:

- minimalist-skill
- soft-skill
- redesign-skill

적용:

- candidate submit form
- knowledge list/detail
- review queue
- source/provenance panel
- approval history

주의:

- approved knowledge와 candidate/draft UI를 섞지 않는다.
- reviewer/admin action은 사용자-facing 화면에 노출하지 않는다.

### Creative Studio

사용 후보:

- taste-skill
- gpt-taste
- image-to-code-skill
- imagegen-frontend-web
- brandkit

적용:

- video storyboard board
- AI presenter landing/portfolio
- executive video visual direction
- media planner intro video visual direction

주의:

- 실제 인물 imitation 금지.
- 내부 화면/캠페인명/광고주명/계정정보 노출 금지.

## 5. Taste Skill을 바로 설치하지 않는 이유

바로 전 제품 repo에 설치하지 않는다.

이유:

- AdMate는 여러 제품 repo가 있어 style drift risk가 크다.
- Taste Skill의 visual variance가 높으면 운영 콘솔 신뢰감을 해칠 수 있다.
- 일부 skill은 landing/creative에는 적합하지만 Sentinel/Engine 같은 운영 UI에는 과할 수 있다.
- 제품별 코드 구현 전에 Design Director의 interpretation layer가 필요하다.

권장:

```text
Design Director Agent가 taste-skill을 reference로 읽고,
제품별 prompt/rules로 번역한 뒤,
각 제품 Agent에게 제한된 Gate로 전달한다.
```

## 6. AdMate 전용 Prompt Translation 예시

### Compass UI redesign prompt seed

```text
Use taste-skill as a design quality reference, but follow AdMate Operational Intelligence Console.
Redesign only the RAG answer/source UI.
Do not modify RAG retrieval, API contract, DB, or environment.
Prioritize evidence readability, source trust, Korean text stability, and calm operational UI.
Avoid cinematic dark, glassmorphism, decorative blobs, and marketing-style hero layout.
```

### Foresight benchmark table prompt seed

```text
Use minimalist/soft taste direction for a benchmark analytics workspace.
Prioritize table readability, metric hierarchy, filter clarity, and report export flow.
Do not expose raw campaign-level data in LLM prompts or UI examples.
Use mock/aggregated data only.
```

### Creative Studio visual exploration prompt seed

```text
Use taste-skill/imagegen-frontend-web as a reference for premium presentation visuals.
Create AdMate executive video storyboard frames and AI presenter visual direction.
Do not imitate real employees.
Do not include internal dashboards, real advertiser names, campaign data, tokens, or private office screens.
```

## 7. Design Director Adoption Gates

### Design-Taste-1 Reference Review

Read and summarize Taste Skill variants. No install. No product repo changes.

### Design-Taste-2 Prompt Translation

Create AdMate-specific taste prompts by product.

### Design-Taste-3 Sandbox PoC

Apply one skill to a non-production mock screen.

### Design-Taste-4 Product Gate

Apply translated prompt to one product surface with explicit constraints.

### Design-Taste-5 QA

Screenshot review, mobile check, text overflow check, accessibility check.

## 8. What Not To Do

- Do not install taste-skill into every product repo by default.
- Do not let a generic premium landing style override AdMate's operational console identity.
- Do not use generated visuals containing real client/campaign data.
- Do not add motion to critical monitoring/approval flows unless it improves clarity.
- Do not modify product logic, RAG, DB, auth, or capture output under a design Gate.

## 9. Current Recommendation

Taste Skill is worth tracking and using as a design acceleration reference.

Recommended adoption:

```text
Use in Design Director Agent first.
Translate into AdMate-specific design prompts.
Pilot on Creative Studio or Homepage.
Then selectively apply to Compass/Foresight/Lens operator UI.
Avoid direct use on Sentinel critical operations until patterns are proven.
```
