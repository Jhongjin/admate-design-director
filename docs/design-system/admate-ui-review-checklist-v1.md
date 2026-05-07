# AdMate UI Review Checklist v1

작성일: 2026-05-07
목적: Design Director가 제품 Agent의 UI 작업을 검토할 때 사용할 공통 checklist를 정의한다.

---

## 1. Scope Check

- [ ] 작업이 디자인, UX, copy, layout, component state에 한정되어 있는가?
- [ ] API contract, DB schema, auth, RAG, capture engine, threshold/model logic이 변경되지 않았는가?
- [ ] copied reference 문서가 수정되지 않았는가?
- [ ] 실제 secret 값, 계정정보, 캠페인 raw data가 포함되지 않았는가?
- [ ] 제품 repo 변경 전 명시적 승인과 작업 범위가 있는가?

---

## 2. Unified Platform Check

- [ ] 배경은 `#F7F7F7` 또는 기존 Openclaw 계열 neutral background와 맞는가?
- [ ] surface/card는 흰색 중심이며 border로 계층을 만드는가?
- [ ] card radius는 기본 8px 이하인가?
- [ ] typography 크기와 density가 운영 콘솔에 맞는가?
- [ ] 제품별 accent가 전체 UI를 지배하지 않는가?
- [ ] 장식 요소가 상태, 근거, audit보다 앞서지 않는가?

---

## 3. Information Architecture Check

- [ ] 첫 viewport에서 현재 상태와 다음 action을 알 수 있는가?
- [ ] 요약/KPI, primary workspace, detail/history가 분리되어 있는가?
- [ ] 중요한 근거는 drawer/detail/source panel에서 추적 가능한가?
- [ ] raw JSON/debug 정보가 기본 노출되지 않는가?
- [ ] 빈 상태, loading, error, partial success 상태가 구분되는가?

---

## 4. Component Check

- [ ] 버튼 문구가 짧은 한국어 동사형인가?
- [ ] destructive/critical action은 확인 단계를 갖는가?
- [ ] 상태는 색상만이 아니라 badge/text로도 구분되는가?
- [ ] table/list row hover, selected, disabled 상태가 있는가?
- [ ] form은 label, helper, error text 구조를 갖는가?
- [ ] icon button에는 tooltip 또는 접근 가능한 label이 있는가?

---

## 5. Korean Text and Responsiveness

- [ ] 긴 한국어 문장이 버튼, badge, table cell, card 안에서 넘치지 않는가?
- [ ] mobile width에서 action button이 겹치지 않는가?
- [ ] source excerpt, alert reason, benchmark basis가 줄바꿈/line-clamp/detail 이동 전략을 갖는가?
- [ ] viewport width에 따라 font-size를 직접 스케일하지 않는가?
- [ ] letter-spacing이 음수로 들어가지 않았는가?

---

## 6. Product Boundary Check

### Compass

- [ ] RAG retrieval, API, DB, search logic을 수정하지 않았는가?
- [ ] source/citation 신뢰도가 visual decoration보다 우선인가?
- [ ] noDataFound, limited evidence, generation failed 상태가 분리되는가?

### Sentinel / Openclaw

- [ ] critical alert와 approval flow에 과한 motion이 없는가?
- [ ] alert severity, suppression, operator action, audit log가 명확한가?
- [ ] monitoring threshold, 권한, Slack action logic이 변경되지 않았는가?

### Lens

- [ ] capture output 자체가 변경되지 않았는가?
- [ ] operator UI와 output fidelity 영역이 분리되어 있는가?
- [ ] quality status, metadata, failure reason이 추적 가능한가?

### Foresight

- [ ] benchmark table과 forecast panel의 읽기 흐름이 명확한가?
- [ ] 최근 데이터 기준, confidence, basis가 표시되는가?
- [ ] raw campaign-level data가 노출되지 않는가?

### Intelligence Library

- [ ] candidate/draft/approved/deprecated 상태가 명확히 구분되는가?
- [ ] source/provenance와 approval history가 보이는가?
- [ ] reviewer/admin action이 사용자-facing 화면에 노출되지 않는가?

---

## 7. Taste Skill / Image Workflow Check

- [ ] Taste Skill이 설치 또는 전면 적용되지 않았는가?
- [ ] AdMate 전용 prompt/rules로 번역된 범위 안에서만 사용되는가?
- [ ] Creative Studio와 Homepage 외 영역에서 image/animation workflow가 제한되었는가?
- [ ] Sentinel critical operations와 Lens capture output에는 직접 적용되지 않았는가?
- [ ] generated visual에 실제 내부 정보가 포함되지 않았는가?

---

## 8. QA Evidence

제품 Agent가 구현한 뒤 제출해야 할 증거:

- [ ] 변경 파일 목록
- [ ] screenshot 또는 visual QA 결과
- [ ] desktop/mobile 확인 결과
- [ ] build/test 결과
- [ ] 기존 기능 불변 확인
- [ ] 접근성/keyboard/focus 기본 확인
- [ ] known risk와 rollback 방법

Design Director 문서 작업만 수행한 경우 제출해야 할 증거:

- [ ] `git diff --check`
- [ ] secret pattern scan
- [ ] large file scan
- [ ] `git status --short`
