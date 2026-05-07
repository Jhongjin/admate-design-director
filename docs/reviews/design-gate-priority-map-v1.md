# Design Gate Priority Map v1

작성일: 2026-05-07
Gate: Design-Director-2
목적: AdMate 제품군의 다음 design gate 후보를 우선순위, 의존성, 허용 범위, 종료 기준으로 정렬한다.

---

## 1. Gate Policy

모든 Gate 공통 원칙:

```text
문서, screenshot audit, prompt/rules, checklist만 작성한다.
제품 repo 코드 수정은 별도 승인 전 금지한다.
API, DB, env, auth, RAG, capture output, monitoring logic은 변경 금지다.
Taste Skill은 설치하지 않고 AdMate 전용 prompt/rules로 번역해서만 사용한다.
Neuform은 template 복사가 아니라 구조/밀도/상태 표현 reference로만 사용한다.
Pretext는 production dependency가 아니라 긴 텍스트 layout stability PoC 후보로만 사용한다.
```

---

## 2. Priority Criteria

우선순위는 아래 기준으로 계산한다.

| Criteria | Weight | Meaning |
|---|---:|---|
| User trust impact | 5 | 사용자 판단과 제품 신뢰에 직접 영향 |
| Output/operation risk | 5 | capture output, critical action, audit 등 사고 가능성 |
| Design drift risk | 4 | 제품군 통일성을 해칠 가능성 |
| Readiness for QA | 3 | 현재 문서 기준으로 바로 audit 가능한가 |
| Implementation blast radius | -4 | 실제 구현이 필요하거나 core logic과 가까울수록 감점 |
| Safe reference applicability | 2 | Neuform/Taste/Pretext를 안전하게 reference로 쓸 수 있는가 |

해석:

- P0: 즉시 audit queue 진입.
- P1: P0 이후 바로 문서/QA Gate 준비.
- P2: sandbox 또는 유지 점검.
- Hold: active issue 없으면 보류.

---

## 3. Ranked Gate Map

| Rank | Gate | Product / Surface | Priority | Allowed Work | Explicitly Forbidden |
|---|---|---|---|---|---|
| 1 | `Gate Design-Lens-Preview-QA-1` | Lens preview UX / operator boundary | P0 | screenshot audit, preview boundary checklist, state matrix | capture output, rendering, injection, composite, platform templates |
| 2 | `Gate Design-Compass-Evidence-QA-1` | Compass source/evidence UX | P0 | source card audit, state matrix, mobile text QA | RAG retrieval, embeddings, ranking, API, DB, auth |
| 3 | `Gate Design-Foresight-Benchmark-QA-1` | Foresight benchmark/report UI | P1 | benchmark IA audit, data trust display matrix | prediction model, data pipeline, raw campaign data |
| 4 | `Gate Design-IntelLib-Candidate-QA-1` | Intelligence Library candidate submit | P1 | candidate/approved state audit, prompt/rules template | reviewer/admin leak, secret/raw data, Taste Skill install |
| 5 | `Gate Design-Homepage-CommandCenter-QA-1` | Homepage / Command Center maintenance | P1 | CTA exposure audit, product card hierarchy, executive dashboard density | internal admin link leak, overclaiming product maturity |
| 6 | `Gate Design-Sentinel-Approval-Audit-QA-1` | Sentinel approval/audit UI | P2 | approval/audit readability, critical action hierarchy notes | threshold, suppression, Slack action, monitoring/auth logic |
| 7 | `Gate Design-CreativeStudio-Sandbox-1` | Creative Studio storyboard sandbox | P2 | sandbox prompt pack, visual safety checklist | real person imitation, internal data, product repo asset commit |
| 8 | `Gate UI-Pretext-1` | Compass or Intelligence Library long text | P2 | CSS-only vs Pretext PoC plan, no production dependency | core dependency adoption without Gate approval |

---

## 4. Gate Details

### 4.1 Gate Design-Lens-Preview-QA-1

Goal:

- Lens preview와 operator controls의 boundary를 확인한다.
- capture output fidelity를 보호하면서 요청/상태/metadata/failure UI만 audit한다.

Inputs:

- Lens UI screenshots or screen recording.
- Current capture request list, detail, quality/failure surfaces.
- Existing Lens UI direction document.

Checklist:

- [ ] preview가 neutral frame 안에 있고 output 자체를 테마화하지 않는다.
- [ ] request/job status가 pending/running/done/review/failure로 명확하다.
- [ ] failure reason과 retry 가능 여부가 분리된다.
- [ ] metadata/detail drawer가 output을 가리지 않는다.
- [ ] regenerate/delete/approve action hierarchy가 안전하다.

Reference use:

- Neuform: image review grid, quality dashboard density.
- Taste Skill: operator UI redesign reference only.
- Pretext: failure reason text overflow가 실제 문제일 때만 낮은 우선순위로 검토.

Exit criteria:

- preview boundary audit note 작성.
- no-touch 영역이 제품 Agent prompt에 포함됨.
- screenshot 기준 P0 visual blocker 목록 정리.

---

### 4.2 Gate Design-Compass-Evidence-QA-1

Goal:

- Compass production-facing RAG UI에서 source/evidence 신뢰도를 QA한다.
- 답변 생성 실패, 검색 실패, limited evidence 상태를 분리한다.

Inputs:

- Compass question/answer/source screenshots.
- noDataFound, limited evidence, generation failed state screenshots.
- Current Compass UI direction document.

Checklist:

- [ ] verified source가 answer card와 연결되어 보인다.
- [ ] source freshness, citation, policy risk가 숨겨지지 않는다.
- [ ] noDataFound는 "AI 답변 실패"와 구분된다.
- [ ] mobile source drawer에서 긴 한국어/URL이 넘치지 않는다.
- [ ] evidence confidence가 색상만으로 표현되지 않는다.

Reference use:

- Neuform: source card, documentation UI, empty state.
- Taste Skill: minimalist/soft/redesign reference only.
- Pretext: long Korean answer/source card PoC 후보.

Exit criteria:

- source/evidence state matrix 작성.
- text overflow risk와 Pretext 필요 여부 판단.
- RAG/API/DB no-touch 문구가 제품 Agent prompt에 포함됨.

---

### 4.3 Gate Design-Foresight-Benchmark-QA-1

Goal:

- Foresight benchmark/report UI의 data trust 구조를 QA한다.
- metric보다 basis/confidence/recent data 기준이 숨겨지지 않게 한다.

Inputs:

- Benchmark table concept or screenshot.
- Forecast panel, report profile, upload mapping flow.
- Current Foresight UI direction document.

Checklist:

- [ ] benchmark table이 platform, industry, objective, period, metric을 비교 가능하게 보여준다.
- [ ] confidence range와 sample/basis가 metric card 주변에 노출된다.
- [ ] 최근 6개월 기준과 장기 추세 참고용이 분리된다.
- [ ] upload validation error가 행/컬럼 단위로 이해된다.
- [ ] mock/aggregate data만 사용된다.

Reference use:

- Neuform: analytics dashboard, table upload flow.
- Taste Skill: minimalist/soft only.
- Pretext: report comments 또는 basis explanation text에 제한 검토.

Exit criteria:

- benchmark/report IA audit 작성.
- data trust display matrix 작성.
- raw campaign-level data 금지 문구가 prompt에 포함됨.

---

### 4.4 Gate Design-IntelLib-Candidate-QA-1

Goal:

- Intelligence Library의 candidate submit, review queue, approved knowledge 상태를 QA한다.
- 제품 Agent에게 전달할 prompt/rules entry template을 만든다.

Inputs:

- Candidate submit flow concept or screenshot.
- Knowledge list/detail and review queue concept.
- Current Intelligence Library UI direction document.

Checklist:

- [ ] candidate/draft/in review/approved/stale/deprecated/blocked 상태가 구분된다.
- [ ] approved knowledge에는 source/provenance가 필수로 보인다.
- [ ] prompt/rules entry가 target product, target surface, no-touch 영역을 포함한다.
- [ ] reviewer/admin actions are not exposed to user-facing surfaces.
- [ ] secret/raw campaign/client/account information 금지 문구가 포함된다.

Reference use:

- Neuform: wiki, review queue, submission form.
- Taste Skill: minimalist/soft/redesign reference only.
- Pretext: long summary and tags/chips layout PoC 후보.

Exit criteria:

- candidate submit UI audit 작성.
- knowledge state matrix 작성.
- prompt/rules entry template 작성.

---

### 4.5 Gate Design-Homepage-CommandCenter-QA-1

Goal:

- Homepage와 Command Center가 AdMate 통합 운영 플랫폼의 첫 인상을 유지하는지 점검한다.
- CTA, product cards, Agent Core 설명, executive read-only dashboard의 노출 범위를 QA한다.

Inputs:

- Homepage and Command Center screenshots.
- Homepage IA and brand copy reference.
- Current design-system document.

Checklist:

- [ ] Hero가 AdMate 정체성을 첫 viewport에서 전달한다.
- [ ] product cards가 제품별 accent를 과하게 쓰지 않는다.
- [ ] Agent Core가 외부 독립 제품처럼 과장되지 않는다.
- [ ] Command Center는 read-only executive surface로 보인다.
- [ ] internal admin links and sensitive actions are not exposed.

Reference use:

- Neuform: clean platform landing, ecosystem cards.
- Taste Skill: restrained polish, hero/section exploration only.
- Pretext: 기본 보류.

Exit criteria:

- CTA exposure matrix 작성.
- product card hierarchy audit 작성.
- Command Center read-only checklist 작성.

---

### 4.6 Gate Design-Sentinel-Approval-Audit-QA-1

Goal:

- Sentinel/Openclaw approval, audit, operator action UI의 readability를 QA한다.
- critical operation을 건드리지 않고 상태/이력/권한 안내만 정리한다.

Inputs:

- Approval queue, audit timeline, operator action log screenshots.
- Current Sentinel/Openclaw UI direction document.

Checklist:

- [ ] critical/warning/normal/suppressed/data unavailable state가 구분된다.
- [ ] critical action은 confirmation과 disabled reason을 가진다.
- [ ] audit log와 operator action이 연결되어 보인다.
- [ ] fail-closed state가 normal state처럼 보이지 않는다.
- [ ] mobile에서 accidental tap risk가 낮다.

Reference use:

- Neuform: incident timeline structure only.
- Taste Skill: non-critical approval/audit readability only.
- Pretext: long alert reason list에 낮은 우선순위.

Exit criteria:

- approval/audit readability audit 작성.
- critical action hierarchy note 작성.
- no-change 영역이 prompt에 포함됨.

---

### 4.7 Gate Design-CreativeStudio-Sandbox-1

Goal:

- Creative Studio에서 image-first workflow를 안전한 sandbox로만 실험한다.
- AdMate executive/media planner intro storyboard를 mock-only로 탐색한다.

Inputs:

- Creative Studio concept brief.
- Taste Skill / AI design workflow notes.
- Visual safety checklist.

Checklist:

- [ ] no real people imitation.
- [ ] no internal dashboards, campaign names, advertiser names, account information.
- [ ] generated visuals are marked mock/sandbox.
- [ ] storyboard does not override operational console identity.
- [ ] no image/video/audio asset is committed to product repo without explicit Gate.

Reference use:

- Neuform: storyboard/presenter landing references.
- Taste Skill: taste/gpt-taste/imagegen reference allowed in sandbox.
- Pretext: script layout only if useful.

Exit criteria:

- sandbox prompt pack 작성.
- visual safety checklist 작성.
- asset storage and review policy 작성.

---

## 5. Design QA Order

권장 순서:

1. Lens preview UX
2. Compass source/evidence UX production QA
3. Foresight benchmark/report UI
4. Intelligence Library candidate submit UI polish
5. Homepage/Command Center 유지 점검
6. Sentinel approval/audit UI
7. Creative Studio sandbox
8. Pretext long-text PoC

왜 이 순서인가:

- Lens는 output fidelity와 operator boundary가 흔들리면 제품 신뢰가 바로 깨진다.
- Compass는 사용자-facing 신뢰 판단이 source/evidence UI에 달려 있다.
- Foresight는 구현이 굳기 전에 benchmark/report information architecture를 고정해야 한다.
- Intelligence Library는 앞으로 Design Director prompt/rules 운영의 기반이다.
- Homepage/Command Center는 유지 점검 성격이 강하다.
- Sentinel은 critical하므로 visual 변경보다 audit readability만 신중하게 본다.
- Creative Studio는 image-first workflow PoC 후보지만 core product QA보다 후순위다.

---

## 6. Gate Output Template

각 Gate 완료 보고는 아래 형식을 따른다.

```text
Gate:
Product:
Surface:
Audit source:
Findings:
P0 blockers:
P1 improvements:
No-touch areas confirmed:
Reference used:
Taste/Neuform/Pretext decision:
Next action:
```

---

## 7. Escalation Rules

즉시 우선순위를 올리는 경우:

- Lens capture output fidelity 문제가 발견됨.
- Compass source/evidence가 잘못된 신뢰 신호를 줌.
- Sentinel critical action 또는 data unavailable state가 normal처럼 보임.
- Homepage/Command Center가 내부 링크 또는 민감 action을 노출함.
- Foresight mock/report에 raw campaign-level data가 포함됨.

Escalation 후에도 금지:

- 제품 repo 코드 수정.
- API/DB/env/auth/RAG/capture/monitoring logic 변경.
- 실제 secret 값 또는 내부 account data 출력.
- AI image/video/audio asset을 제품 repo에 포함.
