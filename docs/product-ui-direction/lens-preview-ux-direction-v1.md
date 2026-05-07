# Lens Preview UX Direction v1

작성일: 2026-05-07
Gate: Design-Director-3
대상: AdMate Lens capture completion preview
범위: 문서 설계만 진행. Lens repo 코드, capture engine, rendering, composite, injection, API, DB, env, 실제 UI 구현은 변경하지 않는다.

---

## 1. Purpose

이 문서는 AdMate Lens에서 캡처 완료 후 결과 이미지를 검토하는 preview 화면을 더 편한 review workspace로 개선하기 위한 디자인 방향이다.

핵심 목표:

```text
캡처 결과 이미지는 크게 본다.
metadata와 action은 우측 inspector에서 빠르게 확인한다.
capture output 자체는 절대 변형하지 않는다.
```

Lens preview UX의 역할은 결과물을 꾸미는 것이 아니라, 사용자가 결과 이미지를 신뢰하고 검토하고 필요한 action을 빠르게 실행하도록 돕는 것이다.

---

## 2. Current Preview UX Problems

현재 이슈:

- preview modal에서 이미지가 너무 작게 보인다.
- metadata가 이미지와 같은 좁은 modal 안에 섞여 있어 확인이 불편하다.
- 사용자는 캡처 결과 이미지를 크게 보고 싶어 한다.
- 사용자는 우측에서 상태, 캡처 시각, duration, result category, placement, landing URL, storage path, diagnostics를 동시에 확인하고 싶어 한다.
- download, copy URL, copy storage path, open original, zoom 같은 action이 preview 흐름 안에서 명확히 묶여 있지 않다.

UX 리스크:

- 작은 preview는 capture output의 실제 품질을 판단하기 어렵게 만든다.
- metadata가 숨어 있으면 실패 원인, placement, 저장 위치를 확인하는 시간이 늘어난다.
- preview와 operator controls가 섞이면 output fidelity 영역과 관리 UI 영역의 책임 경계가 흐려진다.
- preview frame을 과하게 꾸미면 capture output 자체가 AdMate 테마로 바뀐 것처럼 오해될 수 있다.

---

## 3. Design Principles

### 3.1 Image First, Output Untouched

이미지는 가장 크게 보여주되, 실제 output bitmap을 변형하지 않는다.

금지:

- crop, filter, recolor, overlay, border injection, logo/watermark 추가
- output 내부 typography/icon/spacing/layout 변경
- capture 결과를 AdMate UI처럼 재해석하는 frame

허용:

- viewer background
- zoom/pan controls
- neutral checker or light-gray canvas
- image outside frame의 inspector/action UI

### 3.2 Inspector Beside, Not Over

metadata와 action은 이미지 위에 겹치지 않는다. desktop에서는 우측 inspector, mobile에서는 metadata accordion으로 분리한다.

### 3.3 Review Workspace, Not Decorative Modal

preview는 단순 modal보다 작업 공간에 가깝게 설계한다.

포함해야 할 것:

- top toolbar
- large preview area
- placement tabs or thumbnails
- right inspector
- sticky action area
- diagnostics summary

### 3.4 Evidence Before Action

download나 copy보다 먼저 상태, capturedAt, resultCategory, diagnostics가 확인 가능해야 한다.

---

## 4. Desktop Layout Proposal

권장 구조:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│ Top toolbar: back/close | title | status | zoom | open original | download │
├───────────────┬──────────────────────────────────────────────┬─────────────┤
│ Thumbnails /  │                                              │ Inspector   │
│ placement     │              Large preview canvas            │             │
│ tabs          │                                              │ status      │
│               │                                              │ metadata    │
│               │                                              │ actions     │
│               │                                              │ diagnostics │
└───────────────┴──────────────────────────────────────────────┴─────────────┘
```

### 4.1 Large Preview

Large preview area:

- 화면의 가장 큰 영역을 차지한다.
- 원본 비율을 유지한다.
- fit to screen을 기본으로 하고, 사용자가 100%, 150%, 200% zoom을 선택할 수 있게 한다.
- zoom 상태에서는 pan이 가능해야 한다.
- 배경은 neutral light gray 또는 checker pattern을 사용하되, output 자체와 혼동되지 않게 subtle하게 둔다.
- output 주변 frame은 얇은 border나 shadow 수준으로 제한한다.

Recommended desktop sizing:

| Area | Direction |
|---|---|
| Top toolbar | 48px 내외, sticky |
| Left thumbnails/tabs | 96-160px, optional |
| Preview canvas | remaining width, image-first |
| Right inspector | 320-380px |
| Minimum desktop width | 1024px 이상 |

### 4.2 Right Inspector

Right inspector는 image 검토를 방해하지 않는 고정 panel이다.

원칙:

- top에는 status와 핵심 metadata를 둔다.
- action은 metadata 아래 또는 sticky bottom에 둔다.
- diagnostics는 summary를 먼저 보여주고 detail은 accordion으로 접는다.
- storage path와 URL은 줄바꿈과 copy action을 제공한다.

### 4.3 Top Toolbar

Top toolbar는 workspace control 중심이다.

권장 항목:

- close/back
- capture title or placement name
- status badge
- zoom controls
- fit to screen
- open original
- download

주의:

- top toolbar에 모든 action을 넣지 않는다.
- destructive action은 preview toolbar에서 기본 노출하지 않는다.
- download/copy/open 같은 safe action만 toolbar에 둔다.

### 4.4 Thumbnail / Placement Tabs

여러 placement 또는 output variant가 있을 때 사용한다.

옵션:

- left vertical thumbnails
- top placement tabs
- compact segmented control

권장:

- desktop wide layout은 left thumbnails가 좋다.
- placement 수가 적으면 top tabs가 충분하다.
- thumbnail은 output을 crop하지 않고 letterbox로 보여준다.

표시 정보:

- placement label
- device type if available
- status badge or tiny status dot
- generated/captured order

---

## 5. Mobile Layout Proposal

mobile에서는 image first를 유지하되 inspector를 접는다.

권장 구조:

```text
┌──────────────────────────────┐
│ Top bar: close | status      │
├──────────────────────────────┤
│ Large image preview          │
│ pinch/zoom or tap zoom       │
├──────────────────────────────┤
│ Placement tabs / thumbnails  │
├──────────────────────────────┤
│ Metadata accordion           │
│ Diagnostics accordion        │
├──────────────────────────────┤
│ Sticky bottom actions        │
└──────────────────────────────┘
```

### 5.1 Image First

- 첫 화면에서 가능한 한 큰 preview를 보여준다.
- image 아래에 metadata를 길게 펼치지 않는다.
- zoom은 tap 또는 native pinch pattern을 우선한다.
- preview 내부 overlay action은 최소화한다.

### 5.2 Metadata Accordion

mobile metadata는 accordion으로 구성한다.

기본 펼침:

- status
- capturedAt
- resultCategory
- placement

기본 접힘:

- landing URL
- storage path
- diagnostics detail
- durationMs detail

### 5.3 Bottom Actions

mobile action은 sticky bottom bar에 둔다.

권장 순서:

1. download
2. copy URL
3. more actions

More actions:

- copy storage path
- open original
- zoom options

주의:

- button label이 mobile에서 넘치면 icon + accessible label + tooltip/help text로 처리한다.
- copy storage path처럼 긴 결과를 다루는 action은 toast로 성공/실패를 알려준다.

---

## 6. Inspector Panel Information Architecture

Inspector는 아래 순서로 구성한다.

### 6.1 Status

표시:

- capture status badge
- quality/review state if available
- failure/review reason summary if available

권장 state:

| State | Treatment |
|---|---|
| completed | green badge |
| review needed | amber badge |
| failed | red badge |
| processing | info badge |
| unavailable | neutral/amber badge |

### 6.2 Capture Metadata

필수:

- capturedAt
- durationMs
- resultCategory
- placement
- landing URL
- storage path

표시 원칙:

- capturedAt은 local time 기준으로 읽기 쉽게 표시하고 raw timestamp는 detail로 둔다.
- durationMs는 ms와 사람이 읽는 format을 함께 고려한다.
- resultCategory는 badge 또는 compact label로 표시한다.
- landing URL은 hostname을 먼저 보여주고 full URL은 expand/copy로 제공한다.
- storage path는 monospace, line-wrap, copy button을 제공한다.

### 6.3 Placement / Landing URL

placement:

- platform
- placement name
- device type
- creative or format label if available

landing URL:

- hostname
- full URL
- copied state
- unavailable state

주의:

- URL을 자동으로 변형하거나 shorten하지 않는다.
- 사용자가 복사할 수 있게 하되 원본 문자열은 보존한다.

### 6.4 Storage Path

storage path는 운영자-facing 정보다.

권장:

- inspector 하단 또는 metadata section에 배치.
- monospace.
- middle ellipsis 표시 가능.
- copy storage path action 제공.
- full path는 accordion/detail에서 확인.

금지:

- storage path를 output 이미지 위에 overlay.
- secret value나 signed private URL을 그대로 노출.
- path를 사람이 보기 좋게 바꾸면서 원본 식별성을 잃게 만들기.

### 6.5 Diagnostics Summary

diagnostics는 summary first로 보여준다.

권장 구조:

- diagnostics status
- warning count
- error count
- top issue summary
- detail accordion

예시 field:

- render completed
- capture duration warning
- image dimension warning
- storage upload status
- retryable failure

주의:

- diagnostics summary는 pixel fidelity를 보증하는 문구로 쓰지 않는다.
- golden sample 검증이 없으면 "pixel match passed" 같은 표현을 쓰지 않는다.

---

## 7. Action Structure

Action은 safe review actions와 potentially destructive actions를 분리한다.

### 7.1 Primary Review Actions

| Action | Placement | Notes |
|---|---|---|
| download | top toolbar and bottom mobile bar | 현재 표시 중인 output 파일 다운로드 |
| copy URL | inspector action and mobile bottom bar | public or accessible result URL이 있을 때만 |
| copy storage path | inspector metadata row | 운영자-facing action |
| open original | top toolbar or more menu | 새 탭/새 창에서 원본 이미지 열기 |
| zoom | top toolbar and image viewer | fit, 100%, 150%, 200%, reset |

### 7.2 Secondary Actions

조건부로만 노출:

- regenerate
- retry failed capture
- mark for review
- approve result

주의:

- 위 action은 제품 로직/권한과 연결되므로 Design-Director-3 문서에서는 구조만 제안한다.
- 실제 구현 Gate에서는 권한, audit, confirmation, rollback을 별도 확인해야 한다.

### 7.3 Feedback

Action feedback:

- copy success/failure toast
- download started state
- open original unavailable state
- zoom level indicator
- disabled reason text

금지:

- action 성공처럼 보이지만 실제로 실패한 상태를 숨기기.
- storage path copy 실패를 silent fail 처리.
- unavailable URL을 빈 link로 노출.

---

## 8. States and Empty Cases

Preview workspace는 아래 상태를 구분해야 한다.

| State | Direction |
|---|---|
| image ready | large preview + inspector metadata |
| image loading | skeleton or neutral loading state |
| image unavailable | fail-closed message + metadata still visible |
| metadata missing | unavailable badge + missing field label |
| diagnostics warning | amber summary + detail accordion |
| diagnostics failed | red summary + retry guidance if available |
| storage URL missing | copy/open disabled with reason |
| multiple placements | thumbnails/tabs with selected state |

중요:

- image unavailable이어도 metadata를 숨기지 않는다.
- metadata missing을 빈 문자열로 두지 않는다.
- diagnostics warning을 output fidelity pass로 해석하지 않는다.

---

## 9. What Not To Do

절대 하지 않는다.

- capture output image 자체 변형.
- synthetic rendering logic 변경.
- capture engine, rendering, composite, injection 로직 변경.
- platform template, placement fidelity, output typography/icon/layout 변경.
- DB/API/env/auth 변경.
- signed URL, credential, internal token 노출.
- golden sample 없이 pixel fidelity를 통과했다고 주장.
- AI image workflow로 실제 capture output을 대체.
- preview UI 장식으로 output에 watermark, overlay, badge 삽입.

표현 금지:

```text
pixel perfect
pixel match passed
golden sample verified
production fidelity guaranteed
```

위 표현은 실제 golden sample QA와 측정 결과가 있는 Gate에서만 사용할 수 있다.

---

## 10. Design QA Checklist

Desktop:

- [ ] preview image가 화면의 primary area를 차지하는가?
- [ ] right inspector가 이미지 위에 겹치지 않는가?
- [ ] top toolbar action이 safe review action 중심인가?
- [ ] thumbnails/tabs가 output을 crop하지 않는가?
- [ ] inspector width가 metadata readability를 보장하는가?

Mobile:

- [ ] image first flow가 유지되는가?
- [ ] metadata accordion이 너무 길게 첫 화면을 밀어내지 않는가?
- [ ] bottom actions가 겹치거나 text overflow를 만들지 않는가?
- [ ] zoom/open/download action이 mobile에서 reachable한가?
- [ ] storage path와 URL copy feedback이 있는가?

Boundary:

- [ ] output bitmap이 변형되지 않는가?
- [ ] preview frame이 output 내부처럼 보이지 않는가?
- [ ] diagnostics가 pixel fidelity 보증처럼 쓰이지 않는가?
- [ ] missing metadata가 empty state로 명확히 표시되는가?
- [ ] disabled action에는 reason이 있는가?

---

## 11. Lens Agent Implementation Prompt Draft

아래 prompt는 다음 구현 Gate에서 Lens Agent에게 전달할 초안이다. 이 Gate에서는 코드 변경을 실행하지 않는다.

```text
You are working on AdMate Lens preview UX.

Goal:
Improve the completed capture preview into a review workspace:
- large image preview
- right inspector on desktop
- top toolbar
- placement thumbnails or tabs
- mobile image-first layout
- metadata accordion on mobile
- bottom actions on mobile

Hard constraints:
- Do not modify capture output image pixels.
- Do not modify capture engine, rendering, injection, synthetic rendering, composite logic, platform templates, API contracts, DB, auth, or environment.
- Do not claim pixel fidelity or golden sample verification unless an existing golden sample QA result is shown.
- Do not add watermark, overlay, crop, filter, recolor, or visual transformation to the output image.
- Keep preview UI outside the output bitmap.

Inspector fields:
- status
- capturedAt
- durationMs
- resultCategory
- placement
- landing URL
- storage path
- diagnostics summary

Actions:
- download
- copy URL
- copy storage path
- open original
- zoom controls

Desktop layout:
- top toolbar
- large preview canvas
- right inspector around 320-380px
- optional left thumbnails or top placement tabs

Mobile layout:
- image first
- metadata accordion
- diagnostics accordion
- sticky bottom actions

Design style:
- Follow AdMate Operational Intelligence Console.
- Use neutral background, white surface, #E5E5E5 borders, compact typography, and Lens violet only as a muted accent.
- Use badges for completed, review needed, failed, processing, unavailable.
- Keep controls calm and operational.

QA required:
- desktop screenshot
- mobile screenshot
- zoom state
- missing metadata state
- image unavailable state
- multiple placement state if supported
- confirmation that output bitmap is unchanged
- confirmation that API/DB/env/capture logic are unchanged
```

---

## 12. Next Gate Candidate

Recommended next gate:

```text
Gate Design-Lens-Preview-QA-1
```

Expected inputs:

- current Lens preview screenshots
- capture completed state
- capture failed or image unavailable state
- metadata sample with sensitive values removed
- mobile screenshot or viewport capture

Expected outputs:

- screenshot audit notes
- preview boundary issue list
- inspector field availability matrix
- action availability matrix
- implementation prompt finalization
