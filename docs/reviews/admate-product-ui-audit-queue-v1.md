# AdMate Product UI Audit Queue v1

작성일: 2026-05-07
Gate: Design-Director-2
범위: 문서 기반 제품 UI audit queue 작성. 실제 제품 repo 코드, API, DB, env, capture output, RAG 로직은 수정하지 않는다.

---

## 1. Scope and Source

이 문서는 현재 Design Director repo의 reference와 design-system 문서를 기준으로 작성한 UI audit queue다.

참고 기준:

- `docs/design-system/admate-design-system-v1.md`
- `docs/design-system/admate-ui-review-checklist-v1.md`
- `docs/product-ui-direction/*.md`
- `docs/references/neuform-reference-log-v1.md`
- `docs/references/pretext-ui-poc-notes-v1.md`
- `docs/references/taste-skill-ai-design-workflow-notes-v1.md`
- `docs/reference/admate-docs/design/admate-platform-theme-recommendations-v1.md`
- `docs/reference/admate-docs/strategy/13_AdMate_Homepage_IA_Brand_Copy_v1.md`

주의:

- 제품 repo를 live audit한 문서가 아니다.
- "현재 UI 상태"는 reference 문서와 기존 direction 문서 기준의 audit-ready 상태 요약이다.
- 다음 Gate에서 제품 repo를 확인하더라도 우선은 screenshot/QA 중심으로 진행하고, 구현 변경은 별도 승인 없이는 금지한다.

---

## 2. Audit Queue Summary

| Rank | Product / Surface | Priority | Why Now | Next Gate |
|---|---|---|---|---|
| 1 | Lens preview UX / operator boundary | P0 | output fidelity를 보호하면서 preview와 operator controls를 분리해야 한다 | `Gate Design-Lens-Preview-QA-1` |
| 2 | Compass source/evidence UX production QA | P0 | 사용자-facing RAG 신뢰도는 source/citation UI가 결정한다 | `Gate Design-Compass-Evidence-QA-1` |
| 3 | Foresight benchmark/report UI | P1 | PoC/기획 단계에서 데이터 기준과 report 구조를 먼저 고정해야 한다 | `Gate Design-Foresight-Benchmark-QA-1` |
| 4 | Intelligence Library candidate submit UI | P1 | Design prompt/rules 운영을 위해 candidate/approved 상태 모델이 필요하다 | `Gate Design-IntelLib-Candidate-QA-1` |
| 5 | Homepage / Command Center maintenance QA | P1 | 브랜드 허브와 executive read-only surface가 제품군 통일성을 대표한다 | `Gate Design-Homepage-CommandCenter-QA-1` |
| 6 | Sentinel / Openclaw approval/audit UI | P2 | critical operation은 낮은 visual variance로 신중하게 QA해야 한다 | `Gate Design-Sentinel-Approval-Audit-QA-1` |
| 7 | Creative Studio storyboard sandbox | P2 | image-first workflow의 안전한 PoC 후보지만 core 운영 UI보다 후순위다 | `Gate Design-CreativeStudio-Sandbox-1` |

초기 결론:

```text
Lens와 Compass를 먼저 본다.
Lens는 output boundary 리스크가 크고,
Compass는 사용자 신뢰 리스크가 크다.
Foresight와 Intelligence Library는 구조가 굳기 전에 design gate를 잡는다.
Sentinel은 critical UI라서 구현 변동보다 approval/audit readability QA만 제한적으로 진행한다.
Creative Studio는 안전한 sandbox에서만 image-first workflow를 실험한다.
```

---

## 3. Product Audit Matrix

### 3.1 Homepage / Command Center

현재 UI 상태 요약:

- 문서 기준으로 Homepage IA와 brand copy는 정리되어 있다.
- Command Center는 read-only executive dashboard 성격으로, 운영 현황의 구조적 명확성이 중요하다.
- "운영 콘솔의 신뢰감 + 브랜드 전달력" 사이의 균형을 유지해야 한다.

디자인 통일성 리스크:

- 과한 marketing hero, gradient, glass, cinematic dark가 들어오면 AdMate 운영 플랫폼 톤이 흐려진다.
- 제품 카드별 accent가 강해지면 하나의 생태계보다 별도 서비스 묶음처럼 보일 수 있다.
- public/read-only surface에 관리자 action 또는 내부 link가 노출될 위험이 있다.

사용자-facing 우선순위:

- Hero에서 AdMate가 광고 운영 Agent 플랫폼임을 즉시 이해시키기.
- 제품군 관계와 Campaign Lifecycle을 한 viewport 흐름으로 보여주기.
- CTA copy를 내부/외부 노출 수준에 맞춰 분리하기.

운영자-facing 우선순위:

- Command Center product progress cards의 상태/근거/last updated 표시.
- Agent Core, Cost Center, Weekly Intelligence 같은 운영 항목을 과장 없이 표현.
- executive dashboard KPI가 raw data처럼 보이지 않도록 aggregate/mock 기준 표시.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: clean platform landing, ecosystem cards, executive dashboard layout 참고 가능.
- Taste Skill: Homepage/Command Center의 restrained polish에만 사용. image-first exploration은 hero/section concept까지만 허용.
- Pretext: 우선순위 낮음. hero text flow 실험은 가능하지만 운영 신뢰감 저하 시 즉시 제외.

절대 건드리면 안 되는 영역:

- 실제 관리자 link, 신청/권한 action, 내부 route 노출.
- 실제 캠페인명, 광고주명, 계정 정보.
- Openclaw/Hermes를 외부 독립 제품처럼 과장하는 copy.
- 제품 구현 상태를 실제보다 높게 보이게 하는 표현.

Audit checklist:

- [ ] 첫 화면에서 `AdMate`와 `AI Agent 기반 광고 운영 자동화 플랫폼`이 명확한가?
- [ ] 제품군이 Compass/Sentinel/Lens/Foresight/Agent Core로 통일되어 있는가?
- [ ] product card accent가 muted하고 전체 neutral system을 침범하지 않는가?
- [ ] Command Center가 read-only임을 UI action hierarchy가 보여주는가?
- [ ] 내부 운영 link 또는 민감 action이 public surface에 노출되지 않는가?

다음 design gate 후보:

- `Gate Design-Homepage-CommandCenter-QA-1`
- 산출물: screenshot audit notes, CTA exposure matrix, Command Center card hierarchy checklist.

---

### 3.2 Compass

현재 UI 상태 요약:

- Compass는 사용자-facing RAG UI의 신뢰도가 가장 중요하다.
- 답변 카드보다 source/evidence, confidence, policy risk, noDataFound 상태가 먼저 검증되어야 한다.
- 긴 한국어/영어 혼합 정책 문서와 source excerpt가 text overflow 리스크를 만든다.

디자인 통일성 리스크:

- AI 답변 카드가 과하게 chat UI처럼 보이면 근거 신뢰도가 약해진다.
- citation/source card가 제품별 공통 badge, border, typography 규칙에서 벗어날 수 있다.
- empty/error state가 RAG 실패와 검색 실패를 구분하지 못하면 신뢰 리스크가 생긴다.

사용자-facing 우선순위:

- Answer summary, verified source, limited evidence, policy risk를 명확히 표시.
- noDataFound와 generation failed but sources found 상태를 분리.
- mobile source drawer와 긴 source excerpt overflow 확인.

운영자-facing 우선순위:

- document/admin UI는 ingestion/RAG 로직을 건드리지 않는 범위에서 source freshness, status, failure reason만 정리.
- source quality badge와 collected time의 일관성 확인.
- QA history 또는 recent policy question list의 density 확인.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: source card density, documentation UI, empty state 구조 참고 가능.
- Taste Skill: redesign/minimalist/soft reference만 RAG answer/source UI에 제한 적용.
- Pretext: 가장 적합한 PoC 후보. 긴 한국어 answer/source card의 layout stability만 검증.

절대 건드리면 안 되는 영역:

- RAG retrieval, crawler, ingestion, embeddings, reranking, model routing.
- API contract, DB schema, auth, env.
- source truthfulness, confidence calculation, ranking logic.

Audit checklist:

- [ ] source/evidence가 answer보다 신뢰 근거로 먼저 확인 가능한가?
- [ ] verified, partial, stale, unavailable source state가 badge로 구분되는가?
- [ ] noDataFound와 generation failure가 같은 error처럼 보이지 않는가?
- [ ] long Korean/English policy text가 mobile에서 넘치지 않는가?
- [ ] confidence와 policy risk가 decorative color에만 의존하지 않는가?

다음 design gate 후보:

- `Gate Design-Compass-Evidence-QA-1`
- 보조 후보: `Gate UI-Pretext-1`
- 산출물: source/evidence screenshot audit, state matrix, Pretext vs CSS-only text stability note.

---

### 3.3 Sentinel / Openclaw

현재 UI 상태 요약:

- Sentinel/Openclaw는 critical operations와 governance UI가 중심이다.
- reference 기준으로 Live Monitoring, operator actions, audit logs, Slack actions, access governance가 핵심 표면이다.
- visual polish보다 severity clarity, fail-closed state, audit traceability가 우선이다.

디자인 통일성 리스크:

- incident UI에 motion/gradient/glass가 들어오면 운영 판단을 방해한다.
- alert reason, suppression state, operator action이 숨어 있으면 감사성이 약해진다.
- normal state와 data unavailable state가 시각적으로 혼동될 수 있다.

사용자-facing 우선순위:

- Slack/web alert summary에서 severity, reason, next action이 짧게 보이기.
- 보류/오늘 중지/알림 종료/재개 action의 위험도 차이 표시.
- 권한 부족 또는 data unavailable을 명확히 안내.

운영자-facing 우선순위:

- approval queue, audit timeline, operator action log readability.
- fail-closed/security warning surface.
- access request와 governance UI 정리.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: monitoring console, incident timeline 구조 참고 가능.
- Taste Skill: non-critical settings, access request, audit log에만 restrained redesign/minimalist 참고.
- Pretext: alert log의 긴 reason에 낮은 우선순위로만 검토. critical alert 핵심 영역에는 적용 금지.

절대 건드리면 안 되는 영역:

- monitoring threshold, suppression logic, Slack action behavior.
- API contract, DB schema, auth, env.
- audit logging, operator action logging.
- critical alert 판단 흐름에 image-first 또는 animation-heavy workflow 적용.

Audit checklist:

- [ ] critical, warning, normal, suppressed, data unavailable state가 명확히 구분되는가?
- [ ] critical action과 secondary action의 hierarchy가 안전한가?
- [ ] operator action과 audit log가 연결되어 보이는가?
- [ ] data unavailable이 정상/0값처럼 보이지 않는가?
- [ ] approval/audit UI가 제품 공통 badge, table, drawer 규칙을 따르는가?

다음 design gate 후보:

- `Gate Design-Sentinel-Approval-Audit-QA-1`
- 산출물: approval/audit screenshot audit, state wording matrix, critical action hierarchy note.

---

### 3.4 Lens

현재 UI 상태 요약:

- Lens는 capture output fidelity가 제품 가치의 핵심이다.
- 디자인 개선 대상은 capture output이 아니라 preview workspace, request list, quality dashboard, metadata/failure UI다.
- preview와 operator controls의 경계가 가장 먼저 QA되어야 한다.

디자인 통일성 리스크:

- preview를 과하게 꾸미면 output 자체가 AdMate 테마화된 것처럼 보일 수 있다.
- operator UI와 pixel matching 영역이 섞이면 QA 책임 경계가 흐려진다.
- quality status, failure reason, retry action이 약하면 운영자가 결과 신뢰도를 판단하기 어렵다.

사용자-facing 우선순위:

- 캡처 결과 preview를 방해하지 않는 neutral frame.
- 생성 완료/실패/검토 필요 상태의 명확한 badge.
- 결과 metadata와 다운로드/재생성 같은 action hierarchy 정리.

운영자-facing 우선순위:

- capture request queue와 job history density.
- golden sample QA, diff/failure report, retry 가능 여부.
- output fidelity 영역과 operator controls의 시각적 분리.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: quality dashboard, image review grid, job queue 구조 참고 가능.
- Taste Skill: redesign reference는 operator UI에만 허용. image-to-code는 mock operator UI에만 가능.
- Pretext: failure reason 같은 운영 텍스트에 낮은 우선순위로만 검토.

절대 건드리면 안 되는 영역:

- capture output 자체.
- 광고 미리보기 pixel matching 영역.
- rendering, injection, composite engine.
- platform placement fidelity, 결과 이미지 icon/typography/spacing/layout.

Audit checklist:

- [ ] preview와 operator controls가 명확히 분리되는가?
- [ ] output이 AdMate 테마로 재해석된 것처럼 보이지 않는가?
- [ ] quality status와 failure reason이 badge/detail로 추적 가능한가?
- [ ] retry, regenerate, delete, approve action 위험도가 분리되는가?
- [ ] metadata, created time, platform, placement, owner가 확인 가능한가?

다음 design gate 후보:

- `Gate Design-Lens-Preview-QA-1`
- 산출물: preview boundary audit, quality state matrix, failure/retry UX checklist.

---

### 3.5 Foresight

현재 UI 상태 요약:

- Foresight는 benchmark/report UI가 제품 신뢰도의 중심이다.
- 기획/PoC 단계 성격이 강하므로 구현 전에 data basis와 confidence 표현을 고정해야 한다.
- spreadsheet와 analytics console 사이의 dense but readable UI가 필요하다.

디자인 통일성 리스크:

- metric card나 chart가 숫자를 과장하면 근거보다 dashboard 장식이 앞선다.
- benchmark basis, sample count, recent data 기준이 숨겨지면 예측 신뢰도가 약해진다.
- raw campaign-level data가 mock 또는 prompt에 들어갈 위험이 있다.

사용자-facing 우선순위:

- benchmark table과 forecast panel의 비교성.
- confidence range, data basis, recent data 기준의 명확한 노출.
- report export/profile flow의 이해 가능성.

운영자-facing 우선순위:

- upload mapping, rejected rows summary, validation state.
- Net/Gross, markup, currency, period metadata 표시.
- basis drawer와 long report comment의 text stability.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: analytics dashboard, table upload flow 참고 가능.
- Taste Skill: minimalist/soft direction만 benchmark/report UI에 제한 적용.
- Pretext: report comments와 basis 설명의 긴 한국어 preview에 중간 우선순위로 검토.

절대 건드리면 안 되는 영역:

- prediction model, DB schema, data pipeline.
- API contract, auth, env, cost logic.
- raw campaign-level data 노출.
- 최근 6개월 기준과 장기 추세 분리 원칙 훼손.

Audit checklist:

- [ ] metric보다 basis/confidence가 숨겨지지 않는가?
- [ ] benchmark table이 platform, industry, objective, period, metric을 비교 가능하게 보여주는가?
- [ ] upload mapping error가 행/컬럼 단위로 이해되는가?
- [ ] mock/aggregate data만 사용되는가?
- [ ] report profile action이 data trust 설명과 연결되는가?

다음 design gate 후보:

- `Gate Design-Foresight-Benchmark-QA-1`
- 산출물: benchmark/report IA audit, data trust display matrix, upload mapping checklist.

---

### 3.6 Intelligence Library

현재 UI 상태 요약:

- Intelligence Library는 approved knowledge와 candidate/draft/review queue를 분리해야 하는 지식 운영 UI다.
- Design Director가 만든 prompt/rules를 제품 Agent에게 제한적으로 전달하려면 상태 모델과 source/provenance가 중요하다.
- 아직 제품 구현보다 IA와 review workflow 설계가 먼저다.

디자인 통일성 리스크:

- Wiki처럼 읽기만 좋은 UI가 되면 approval/audit이 약해진다.
- candidate와 approved가 섞이면 Agent 참조 기준이 불안정해진다.
- reviewer/admin action이 사용자-facing surface에 노출될 수 있다.

사용자-facing 우선순위:

- knowledge list/detail의 읽기 흐름.
- approved/stale/deprecated 상태의 명확한 표시.
- source/provenance panel의 접근성.

운영자-facing 우선순위:

- candidate submit form, review queue, approval history.
- prompt/rules library의 target product, allowed surface, no-touch 영역 표시.
- blocked/deprecated 지식의 사용 금지 안내.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: wiki, review queue, submission form 구조 참고 가능.
- Taste Skill: minimalist/soft/redesign direction을 candidate submit, list/detail에 제한 적용.
- Pretext: 긴 summary, tags/chips, provenance text layout에 중간 우선순위로 검토.

절대 건드리면 안 되는 영역:

- approved/candidate state logic을 시각적으로 혼동시키는 디자인.
- reviewer/admin action의 사용자-facing 노출.
- secret 값, raw campaign data, private client/account information.
- Taste Skill 원본 설치 또는 제품 repo 직접 적용.

Audit checklist:

- [ ] candidate, draft, in review, approved, stale, deprecated, blocked 상태가 구분되는가?
- [ ] approved knowledge는 source/provenance 없이 표시되지 않는가?
- [ ] prompt/rules entry에 target product와 no-touch 영역이 포함되는가?
- [ ] reviewer/admin action 노출 범위가 분리되는가?
- [ ] 긴 한국어 summary와 tags/chips가 mobile에서 깨지지 않는가?

다음 design gate 후보:

- `Gate Design-IntelLib-Candidate-QA-1`
- 보조 후보: `Gate UI-Pretext-1`
- 산출물: candidate submit audit, knowledge state matrix, prompt/rules entry template.

---

### 3.7 Creative Studio

현재 UI 상태 요약:

- Creative Studio는 image-first, storyboard, presenter visual direction을 실험할 수 있는 가장 안전한 후보 영역이다.
- core 운영 UI가 아니라 sandbox PoC로 다뤄야 한다.
- 내부 정보와 실제 인물 imitation을 배제해야 한다.

디자인 통일성 리스크:

- premium visual exploration이 AdMate 전체 운영 콘솔 톤을 덮어쓸 수 있다.
- generated visual이 실제 dashboard, 캠페인명, 광고주명, 사내 정보처럼 보일 수 있다.
- 영상/animation workflow가 Sentinel/Lens 같은 금지 영역으로 확산될 위험이 있다.

사용자-facing 우선순위:

- executive/media planner intro storyboard의 message clarity.
- AdMate brand와 제품군 관계를 과장 없이 전달.
- generated visual은 mock-only로 명확히 표시.

운영자-facing 우선순위:

- storyboard board, script blocks, review status, approved frame list.
- asset safety checklist와 source disclosure.
- image/video prompt에 금지 정보가 들어가지 않는지 확인.

Neuform / Taste Skill / Pretext 적용 가능성:

- Neuform: storyboard/presenter landing, visual exploration 참고 가능.
- Taste Skill: taste-skill, gpt-taste, image-to-code, imagegen-frontend-web, brandkit reference 가능.
- Pretext: storyboard script text layout 실험 정도로만 낮은 우선순위.

절대 건드리면 안 되는 영역:

- 실제 인물 imitation.
- 내부 dashboard, 캠페인명, 광고주명, 계정정보, token, private office screen.
- Sentinel critical operations, Lens capture output으로 image/animation workflow 확산.
- 제품 repo에 Taste Skill 설치.

Audit checklist:

- [ ] visual prompt에 실제 내부 정보가 포함되지 않는가?
- [ ] generated visual이 mock/sandbox임을 명확히 표시하는가?
- [ ] storyboard가 AdMate 제품군 메시지를 왜곡하지 않는가?
- [ ] image-first workflow가 Creative Studio 밖으로 확산되지 않도록 Gate가 있는가?
- [ ] asset 생성/저장/검토 위치가 제품 repo와 분리되어 있는가?

다음 design gate 후보:

- `Gate Design-CreativeStudio-Sandbox-1`
- 산출물: sandbox prompt pack, visual safety checklist, storyboard frame review rubric.

---

## 4. Cross-Product Audit Checklist

모든 제품 Gate에서 공통 확인:

- [ ] AdMate Operational Intelligence Console 기준을 따른다.
- [ ] 제품별 accent는 muted하고 상태 판단을 방해하지 않는다.
- [ ] background, surface, border, typography, badge, button 규칙이 통일되어 있다.
- [ ] 상태, 근거, 승인, audit, source 중 해당 제품의 핵심 정보가 숨겨지지 않는다.
- [ ] 긴 한국어 문장과 mobile layout이 깨지지 않는다.
- [ ] 제품 로직, API, DB, auth, RAG, capture output은 변경하지 않는다.
- [ ] 실제 campaign raw data, secret, private account information을 예시로 쓰지 않는다.
- [ ] Taste Skill, Neuform, Pretext는 reference 또는 PoC Gate로만 사용한다.
- [ ] AI image/video/animation workflow는 Creative Studio 중심으로 제한한다.

---

## 5. Recommended Immediate Queue

1. `Gate Design-Lens-Preview-QA-1`
2. `Gate Design-Compass-Evidence-QA-1`
3. `Gate Design-Foresight-Benchmark-QA-1`
4. `Gate Design-IntelLib-Candidate-QA-1`
5. `Gate Design-Homepage-CommandCenter-QA-1`
6. `Gate Design-Sentinel-Approval-Audit-QA-1`
7. `Gate Design-CreativeStudio-Sandbox-1`

Escalation rule:

- 실제 운영 사고, auth/security issue, capture output fidelity issue가 발견되면 Sentinel 또는 Lens Gate를 즉시 P0로 올린다.
- 단, escalation이 있어도 Design Director Gate는 문서/QA/prompt/rules 범위를 넘지 않는다.
