# Lens UI Review Prompt v1

작성일: 2026-05-07
Gate: Design-Director-2
대상: AdMate Lens 제품 Agent
범위: UI review, screenshot audit, prompt/rules 전달용 템플릿. 제품 repo 코드 수정은 별도 승인 전 금지한다.

---

## 1. UI 목적

Lens는 광고 게재 화면과 보고서 증빙 이미지를 자동 생성하는 Capture Automation 제품이다.

Lens UI의 목적은 capture output을 꾸미는 것이 아니라, 운영자가 결과 이미지의 상태, metadata, failure reason, review action을 빠르게 확인하도록 돕는 것이다.

핵심 기준:

```text
Lens는 캡처와 증빙을 만든다.
Capture output bitmap은 불변이다.
Preview workspace는 output을 크게 보여주고 inspector/action을 분리한다.
```

---

## 2. 반드시 지켜야 할 디자인 원칙

- AdMate Operational Intelligence Console 기준을 따른다.
- capture output image 자체는 절대 변형하지 않는다.
- preview와 operator controls는 시각적으로 분리한다.
- completed, review needed, failed, processing, unavailable 상태를 badge로 명확히 구분한다.
- metadata, diagnostics, storage path, landing URL은 우측 inspector 또는 mobile accordion에서 확인한다.
- download, copy URL, copy storage path, open original, zoom은 safe review action으로 묶는다.
- Lens violet accent는 muted하게 사용하고 output보다 강하게 보이지 않게 한다.

---

## 3. 수정 금지 영역

제품 Agent는 아래를 수정하지 않는다.

- capture output bitmap
- 광고 미리보기 pixel matching 영역
- rendering, injection, synthetic rendering, composite engine
- platform templates and placement fidelity
- output image 내부 icon, typography, spacing, layout
- capture engine logic
- API contract
- DB schema
- auth/session/permission
- environment variables
- signed URL, token, credential 노출
- copied reference 원문

허용되는 범위:

- request list UI review
- preview workspace IA review
- metadata inspector layout review
- quality/failure state review
- operator action hierarchy review
- screenshot audit notes

---

## 4. 우선 점검 화면/컴포넌트

| Surface | Review Focus |
|---|---|
| Capture request list | status, platform, placement, owner, created time이 보이는가 |
| Capture completed preview | image-first workspace이며 output이 변형되지 않는가 |
| Right inspector | status, capturedAt, durationMs, resultCategory, placement, landing URL, storage path가 보이는가 |
| Diagnostics summary | warning/error count와 top issue가 summary-first로 보이는가 |
| Quality dashboard | golden sample, diff status, pass/fail/review badge가 과장 없이 보이는가 |
| Failure report | failure reason, retry 가능 여부, affected placement가 분리되는가 |
| Mobile preview | image first, metadata accordion, sticky safe actions가 안정적인가 |

---

## 5. 좋은 상태 / 나쁜 상태 기준

좋은 상태:

- output image가 가장 큰 영역에 원본 비율로 표시된다.
- inspector가 이미지 위에 overlay되지 않는다.
- status와 diagnostics를 본 뒤 action을 실행할 수 있다.
- storage path와 URL은 copy action과 unavailable/disabled reason을 가진다.
- image unavailable이어도 metadata는 가능한 범위에서 계속 표시된다.
- retry/regenerate/delete/approve 같은 action은 위험도와 권한에 따라 분리된다.

나쁜 상태:

- output image에 watermark, overlay, filter, crop, recolor, AdMate badge가 들어간다.
- preview frame이 output 내부 UI처럼 보인다.
- metadata와 action이 이미지 위를 가린다.
- diagnostics warning을 pixel fidelity pass처럼 표현한다.
- signed URL, credential, private token처럼 보이는 값이 그대로 노출된다.
- capture engine 또는 platform template을 UI polish 목적으로 바꾼다.

---

## 6. 모바일 / 데스크톱 체크 포인트

Desktop:

- top toolbar, large preview canvas, right inspector가 명확히 분리된다.
- right inspector는 320-380px 수준의 metadata readability를 갖는다.
- zoom controls, fit, open original, download가 output을 가리지 않는다.
- placement thumbnails/tabs가 output을 crop하지 않고 letterbox로 보여준다.

Mobile:

- 첫 화면에서 image preview가 primary area를 차지한다.
- metadata와 diagnostics는 accordion으로 접고 펼칠 수 있다.
- sticky bottom actions가 버튼 text overflow나 accidental tap risk를 만들지 않는다.
- copy/open/download 실패 시 toast 또는 disabled reason이 있다.

---

## 7. 긴 한국어 텍스트 안정성 기준

- failure reason과 diagnostics summary는 2-3줄 요약 후 detail accordion으로 보낸다.
- landing URL과 storage path는 monospace, line-wrap, copy action을 제공한다.
- 긴 placement명이나 result category가 badge/button 안에서 넘치지 않아야 한다.
- mobile에서 `복사`, `다운로드`, `원본 열기` 등 action label이 겹치면 icon + accessible label을 사용한다.
- storage path는 사람이 보기 좋게 변형하더라도 원본 식별성을 잃지 않게 detail에서 full value를 확인한다.

---

## 8. Badge / Source / Audit / Confidence / Provenance 기준

상태 badge:

- `Completed`: green
- `Review needed`: amber
- `Failed`: red
- `Processing`: info
- `Unavailable`: neutral/amber

Source:

- Lens의 핵심 source는 output file, landing URL, storage path, platform/placement metadata다.
- output file provenance와 capture timestamp를 inspector에서 확인한다.

Audit:

- created time, capturedAt, owner, retry/regenerate/approve/delete action history를 추적 가능하게 한다.
- destructive action은 confirmation과 audit preview가 있어야 한다.

Confidence:

- diagnostics를 confidence처럼 과장하지 않는다.
- golden sample QA 결과가 없으면 pixel match passed 또는 production fidelity guaranteed를 쓰지 않는다.

Provenance:

- output이 어떤 request, placement, landing URL, storage path에서 생성되었는지 연결한다.
- missing metadata는 빈 문자열이 아니라 unavailable state로 표시한다.

---

## 9. 제품 Agent 전달용 실행 프롬프트

```text
You are reviewing AdMate Lens UI.

Goal:
Audit the Lens operator UI and completed capture preview workspace while protecting capture output fidelity.

Scope:
- capture request list
- completed preview workspace
- right inspector metadata
- diagnostics summary
- quality dashboard
- failure report
- mobile image-first preview
- action hierarchy for download, copy URL, copy storage path, open original, zoom

Hard constraints:
- Do not modify product code unless explicitly approved in a later implementation Gate.
- Do not modify capture output pixels, preview fidelity, rendering, injection, synthetic rendering, composite logic, platform templates, API contracts, DB schema, auth, environment, or secrets.
- Do not add watermark, overlay, crop, filter, recolor, or visual transformation to the output image.
- Do not claim pixel fidelity or golden sample verification unless an existing measured QA result is provided.
- Do not edit copied reference documents.
- Do not call external APIs.
- Do not expose signed URLs, credentials, tokens, real campaign names, advertiser names, or account information.

Design direction:
- Follow AdMate Operational Intelligence Console.
- Make the image preview large and neutral.
- Place metadata and actions in a right inspector on desktop and accordions/bottom actions on mobile.
- Prioritize job status clarity, quality review state, failure reason, metadata provenance, and safe review actions.

Review output:
- preview boundary audit
- P0/P1 UI issues
- quality/failure state matrix
- desktop and mobile screenshot notes
- confirmation that output bitmap and capture logic are untouched
- recommended prompt/rules for a future implementation Gate
```
