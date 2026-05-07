# Lens UI Direction v1

작성일: 2026-05-07
대상: AdMate Lens, Capture Automation
목적: Lens의 캡처 결과물 정확도를 보호하면서 운영자 UI만 AdMate 공통 디자인으로 정리하기 위한 방향을 정의한다.

---

## 1. Product Role

Lens는 광고 게재 화면과 보고서 증빙 이미지를 자동 생성한다.

한 줄 기준:

```text
Lens는 캡처와 증빙을 만든다.
```

Lens에서 가장 중요한 것은 output fidelity다. 광고 미리보기, 합성 결과, 캡처 결과물은 실제 매체 화면과 픽셀 매칭이 중요하므로 디자인 실험 대상이 아니다.

---

## 2. Boundary

디자인 적용 가능:

- 관리자 화면
- 캡처 요청 폼
- 캡처 요청 목록
- 작업 이력
- 품질 상태 dashboard
- golden sample QA 화면
- 결과 metadata/detail modal
- failure report UI

디자인 적용 금지:

- capture output 자체
- 광고 미리보기의 pixel matching 영역
- rendering/injection/composite engine
- platform placement fidelity
- 결과 이미지의 icon, typography, spacing, layout

---

## 3. Design Objective

Lens 운영자 UI의 목표:

- 어떤 캡처가 요청/진행/완료/실패 상태인지 빠르게 알 수 있다.
- output fidelity 검수 상태와 failure reason을 추적할 수 있다.
- 결과물 metadata와 재생성 action이 명확하다.
- AdMate 공통 운영 콘솔 톤으로 관리 화면이 정돈된다.

추천 조합:

```text
Linear + Sentry + minimal Apple-like spacing
```

AdMate 번역:

```text
작업 큐의 정밀함
+ 오류/품질 추적성
+ 결과물 확인을 방해하지 않는 여백
```

---

## 4. Primary Surfaces

| Surface | Direction |
|---|---|
| Capture request list | status, platform, placement, created time, owner를 table/list로 표시 |
| Request form | platform, placement, creative input, output format을 단계적으로 구성 |
| Capture detail | preview는 보호하고 metadata/action/history를 주변에 배치 |
| Quality dashboard | golden sample, diff status, pass/fail/review badge |
| Failure report | failure reason, retry 가능 여부, affected placement 표시 |

---

## 5. Visual Direction

기본:

- AdMate neutral background와 white surface를 사용한다.
- result preview는 과한 frame으로 꾸미지 않는다.
- 품질 상태는 badge와 compact table로 보여준다.

Accent:

- Lens accent: violet
- quality ok: green
- review: amber
- failure: red

권장 구조:

```text
Topbar
Capture queue summary
Request list / Quality list
Selected capture detail drawer
Metadata / History / Failure reason
```

---

## 6. Taste Skill Translation

Lens에는 Taste Skill을 output 영역에 직접 적용하지 않는다.

허용:

- redesign-skill reference for operator UI
- image-to-code-skill reference for operator UI mock only

금지:

- capture output redesign
- generated image로 실제 매체 UI 대체
- output typography/icon/layout 변경
- image/animation workflow로 report proof를 장식화

제품 Agent 전달 prompt seed:

```text
Follow AdMate Operational Intelligence Console.
Redesign only Lens operator UI: request list, request form, job history, quality status, metadata detail, and failure report.
Do not modify capture output, preview fidelity, rendering, injection, composite logic, platform templates, API contracts, DB, auth, or environment.
Keep output preview visually neutral and protect pixel matching.
Prioritize job status clarity, quality review state, failure reason, and retry workflow.
```

---

## 7. QA Focus

Lens UI review는 다음을 확인한다.

- capture result가 디자인 작업으로 바뀌지 않았는가?
- preview와 operator controls가 시각적으로 분리되어 있는가?
- failure reason이 운영자가 이해할 수 있는 한국어로 표시되는가?
- 재생성/삭제/승인 같은 action이 위험도에 맞게 구분되는가?
- result metadata와 audit history가 추적 가능한가?
