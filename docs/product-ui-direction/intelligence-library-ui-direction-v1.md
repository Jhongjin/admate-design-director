# Intelligence Library UI Direction v1

작성일: 2026-05-07
대상: AdMate Intelligence Library / Wiki
목적: 사람이 읽는 Wiki와 Agent가 참조하는 approved knowledge layer를 분리하면서, 지식 승인/출처/상태를 추적 가능한 UI로 설계한다.

---

## 1. Product Role

Intelligence Library는 AdMate의 운영 지식, 승인된 기준, reference, prompt/rules 후보를 관리하는 지식 레이어다.

핵심:

```text
사람이 읽는 Wiki
+ Agent가 참조하는 approved knowledge
+ 후보/검토/승인/폐기 흐름
```

---

## 2. Design Objective

Intelligence Library UI의 목표:

- approved knowledge와 candidate/draft를 명확히 구분한다.
- source/provenance와 approval history를 항상 추적 가능하게 한다.
- 제품 Agent에게 전달할 prompt/rules의 상태와 적용 범위를 명확히 한다.
- stale/deprecated 지식이 최신 기준처럼 보이지 않게 한다.

추천 조합:

```text
Notion + Mintlify + Linear + Sentry
```

AdMate 번역:

```text
읽기 좋은 knowledge UI
+ 문서/출처 신뢰성
+ review queue의 workflow clarity
+ 상태/이력 추적성
```

---

## 3. Primary Surfaces

| Surface | Direction |
|---|---|
| Knowledge list | type, product, status, owner, updated time, source count |
| Knowledge detail | summary, full content, source/provenance, related products |
| Candidate submit | 신규 reference/prompt 후보 제출 |
| Review queue | reviewer action, approval state, risk note |
| Source panel | URL/file reference, imported time, confidence |
| Prompt/rules library | 제품별 Agent 전달 prompt와 constraints |
| Approval history | 누가 승인/수정/폐기했는지 기록 |

---

## 4. Visual Direction

기본:

- 읽기 좋은 document layout
- compact metadata rail
- status badge와 source count 표시
- review queue는 table/list 중심

Accent:

- Intelligence Library accent: slate-indigo
- approved: green
- review: blue/amber
- stale: amber
- deprecated: gray
- blocked: red

권장 구조:

```text
Topbar
Knowledge filters
List or queue
Detail reading pane
Source/provenance panel
Approval history
```

---

## 5. Knowledge States

| State | Meaning | UI Treatment |
|---|---|---|
| Candidate | 제안된 지식/프롬프트 | info badge, review CTA |
| Draft | 작성 중 | gray/info badge |
| In Review | 검토 중 | amber badge |
| Approved | Agent 참조 가능 | green badge, source/provenance 필수 |
| Stale | 최신성 확인 필요 | amber badge, 사용 경고 |
| Deprecated | 더 이상 사용하지 않음 | gray badge, replacement link |
| Blocked | 보안/정확성 문제 | red badge, 사용 금지 |

---

## 6. Taste Skill Translation

Intelligence Library에는 minimalist/soft/redesign 방향만 제한적으로 적용한다.

허용:

- candidate submit form
- knowledge list/detail
- review queue
- source/provenance panel
- prompt/rules library

제한:

- approved knowledge와 candidate UI를 섞지 않는다.
- reviewer/admin action은 일반 사용자 화면에 노출하지 않는다.
- Taste Skill은 설치하지 않고 AdMate 전용 prompt/rules로 번역된 결과만 저장한다.

제품 Agent 전달 prompt seed:

```text
Use minimalist/soft taste direction only as a reference.
Follow AdMate Operational Intelligence Console.
Design Intelligence Library as a knowledge and review workspace.
Separate candidate, draft, approved, stale, deprecated, and blocked states clearly.
Do not expose admin/reviewer actions to user-facing views.
Do not include secret values, raw campaign-level data, private client names, or account information.
Prioritize source/provenance, approval history, Korean readability, and Agent prompt/rules scope clarity.
```

---

## 7. QA Focus

- approved와 candidate가 시각적으로 혼동되지 않는가?
- stale/deprecated 지식이 최신 기준처럼 보이지 않는가?
- source/provenance 없이 approved 처리되지 않는가?
- prompt/rules에 제품별 금지 영역이 포함되는가?
- 사용자 권한별로 action 노출이 분리되는가?
