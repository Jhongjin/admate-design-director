# Auth / Account UX Direction v1

작성일: 2026-05-07
Gate: Design-Director-4
대상: AdMate Homepage, Compass, Lens, Sentinel / Agent Core, Foresight
범위: 로그인, 이용 신청, 비밀번호 설정/재설정, 프로필, 권한 상태, 권한 요청 화면의 공통 UX/UI 기준. 제품 repo 코드와 실제 auth 구현은 변경하지 않는다.

---

## 1. Purpose

AdMate는 Compass, Lens, Sentinel / Agent Core, Foresight, Homepage로 나뉘어 있지만 사용자는 하나의 AdMate 계정 경험을 기대한다.

현재 로그인, 회원가입, 권한 신청이 Sentinel / Agent Core 맥락 안에 섞여 보이면 Compass나 Lens 사용자는 "내가 다른 제품에 잘못 들어왔나"라고 느낄 수 있다. 따라서 기능 구현 전에 공통 auth/account 경험의 디자인 기준을 분리한다.

핵심 목표:

```text
AdMate account experience
= Homepage에서 이용 신청
+ 제품별 로그인은 같은 구조와 제품별 얇은 accent
+ 일반 사용자는 소속, 팀, 권한 상태, 접근 가능 제품만 명확히 확인
+ 내부 관리자는 Agent Core / Sentinel 콘솔에서 audit과 authority를 상세 확인
```

---

## 2. Product Boundary

이 문서는 UI/UX와 prompt/rules 설계만 다룬다.

금지:

- 제품 repo 코드 수정
- 실제 로그인/회원가입/권한 신청 구현
- API, DB, auth, env 변경
- 권한 모델, role calculation, session behavior 변경
- email delivery, password reset token, invite token 구현 변경
- copied reference 원문 수정
- secret, token, credential 출력

허용:

- 화면 IA 설계
- layout, copy, state, component 기준 정의
- 제품별 login variant 방향
- permission status component 기준
- 제품 Agent에게 전달할 UI-only implementation prompt 작성

---

## 3. Design Principles

### 3.1 AdMate First

사용자-facing 화면은 `AdMate 로그인`, `AdMate Compass 로그인`, `AdMate Lens 로그인`처럼 AdMate와 제품 맥락을 유지한다.

Openclaw와 Hermes는 일반 사용자 login/account 화면의 전면 브랜드가 아니다. 필요한 경우 내부 운영 설명에서만 Agent Core의 engine으로 다룬다.

### 3.2 Homepage As Request Entry

가입과 이용 신청의 시작점은 Homepage 중심으로 둔다. 제품별 login 화면은 이미 계정이 있거나 초대받은 사용자의 진입점으로 보이게 한다.

### 3.3 Shared Structure, Thin Product Accent

제품별 로그인은 같은 layout, input, help, status 규칙을 공유한다. 차이는 제품 label, 작은 accent line, icon tint, 도움말 copy 정도로 제한한다.

### 3.4 Permission Clarity

일반 사용자는 아래만 명확히 본다.

- 이름 또는 이메일
- 소속
- 팀
- 계정 상태
- 접근 가능 제품
- pending/rejected reason summary
- 추가 플랫폼 요청 action

내부 관리자는 Agent Core / Sentinel 콘솔에서 더 상세한 audit, authority, approval history를 본다.

### 3.5 Calm Operational Form

auth/account 화면도 AdMate Operational Intelligence Console의 일부다.

- app background: `#F7F7F7`
- surface: `#FFFFFF`
- border: `#E5E5E5`
- radius: 8px 이하
- body: 13-14px
- helper: 11-12px
- shadow보다 border와 spacing
- 제품 accent는 muted하게 제한

---

## 4. Auth / Account IA

권장 IA:

```text
Homepage
  -> 이용 신청
  -> 신청 완료 / pending 안내

Product login
  -> 로그인
  -> 비밀번호 설정 / 재설정 안내
  -> 로그인 성공
      -> 권한 확인
          -> 접근 가능: 제품 workspace
          -> pending: 권한 상태 화면
          -> rejected: 재요청 안내
          -> no access: 추가 플랫폼 요청

Profile / Account
  -> 기본 정보
  -> 소속 / 팀
  -> 접근 가능 제품
  -> 권한 요청 상태
  -> 추가 플랫폼 요청
  -> 보안/비밀번호 도움말

Admin review
  -> Agent Core / Sentinel console
  -> approval queue
  -> audit / authority detail
```

사용자-facing IA와 admin IA를 분리한다.

| Audience | Primary Surface | Visible Information |
|---|---|---|
| Public visitor | Homepage | AdMate 설명, 이용 신청 entry, 문의/CTA |
| Invited user | Product login | 제품 맥락 login, password setup/reset |
| General user | Profile / Account | 소속, 팀, 권한 상태, 접근 가능 제품 |
| Pending user | Request status | 신청 접수, 검토 상태, 예상 next step |
| Rejected user | Request status | 거절 summary, 재요청 또는 문의 |
| Internal admin | Agent Core / Sentinel | approval, authority, audit, operator history |

---

## 5. Homepage 이용 신청 Entry

Homepage의 이용 신청 entry는 public-safe CTA다.

권장 위치:

- Hero secondary CTA
- Product cards 하단
- Final CTA
- Command Center에는 read-only viewing 맥락일 때만 제한적으로 배치

권장 label:

- `이용 신청`
- `AdMate 사용 요청`
- `권한 요청하기`
- `도입 문의`

권장 flow:

```text
CTA 선택
-> 이용 신청 form 또는 request landing
-> 이름, 회사/부서, 팀, 업무 목적, 희망 제품 선택
-> 제출
-> pending 안내
```

주의:

- 신청 CTA가 즉시 사용 가능처럼 보이면 안 된다.
- 내부 admin route나 approval action을 public page에 노출하지 않는다.
- 실제 계정정보, 광고주명, 캠페인 raw data를 예시로 쓰지 않는다.

---

## 6. 공통 Login Layout

기본 구조:

```text
Top minimal brand bar
  AdMate / Product label

Main area
  Left or top context block
    Product name
    Short product role
    Access status hint

  Login surface
    Email input
    Password input
    Primary login button
    Password setup/reset link
    Request access link

  Help surface
    계정/권한 안내
    pending/rejected/no access state link
```

Desktop:

- max-width 960-1080px
- login surface width 360-420px
- context block은 짧고 compact하게 둔다.
- decorative illustration보다 product role과 account guidance를 우선한다.

Mobile:

- single column
- login surface first
- product context는 login 위 2-3줄로 제한
- request/access help는 accordion 또는 compact link group
- input, button, status badge가 겹치지 않아야 한다.

Common component rules:

- input height: 34-40px
- button height: 34-40px
- radius: 6-8px
- label은 input 위에 명확히 배치
- error/helper text는 input 바로 아래
- password visibility toggle은 icon + accessible label
- keyboard focus state 필수

---

## 7. 제품별 Login Variant

제품별 variant는 얇은 accent와 제품 role copy만 다르게 한다.

| Product | Login Title | Accent | Context Copy Direction |
|---|---|---|---|
| Compass | `AdMate Compass 로그인` | indigo-blue | 정책과 가이드 근거 확인 |
| Lens | `AdMate Lens 로그인` | violet | 캡처 요청과 증빙 관리 |
| Sentinel / Agent Core | `AdMate Sentinel 로그인` 또는 `AdMate Agent Core 관리자 로그인` | emerald or graphite | 운영 감지, 승인, audit 콘솔 |
| Foresight | `AdMate Foresight 로그인` | amber / analytic blue | benchmark와 성과 예측 |
| Generic | `AdMate 로그인` | neutral/info | 제품 선택 전 공통 로그인 |

주의:

- 일반 사용자에게 `Openclaw 로그인`이라고 표시하지 않는다.
- 일반 사용자에게 `Hermes 권한`, `Hermes 관리자`를 전면 노출하지 않는다.
- Sentinel / Agent Core 관리 화면은 내부 관리자 맥락에서만 `Agent Core 관리자 로그인`을 쓸 수 있다.
- 제품별 accent는 top border, selected product chip, icon tint 정도로 제한한다.

---

## 8. Password Setup / Reset 안내 화면

목적:

- 초대받은 사용자가 비밀번호를 설정한다.
- 기존 사용자가 비밀번호 재설정 안내를 받는다.
- reset link 만료/오류 상태를 안전하게 안내한다.

권장 화면:

| State | UI Direction |
|---|---|
| Setup ready | 이메일 확인, 새 비밀번호 입력, 확인 입력 |
| Setup complete | 로그인으로 이동, 접근 가능 제품 안내 |
| Reset requested | 이메일 발송 안내, inbox 확인 |
| Link expired | 재요청 CTA, 보안상 상세 token 정보 비노출 |
| Invalid request | 도움말, 이용 신청/문의 link |
| Password policy error | 조건 checklist, 입력 field 아래 error |

Copy 원칙:

- token, internal auth provider, DB row 같은 내부 정보를 노출하지 않는다.
- 보안 오류를 너무 상세하게 설명하지 않는다.
- "메일이 없으면 이용 신청 상태를 확인해 주세요"처럼 next action을 준다.

---

## 9. Profile / Account Page IA

Profile / Account는 일반 사용자의 계정 상태 확인 화면이다.

권장 구조:

```text
Topbar
  Account title
  Last updated

Profile summary
  Name / Email
  Organization
  Team
  Account status badge

Accessible products
  Product cards or compact list
  Status per product
  Primary product link

Permission requests
  Pending / approved / rejected history summary
  Request additional platform

Security help
  Password reset
  Contact admin/help
```

일반 사용자에게 보이는 정보:

- name/email
- organization
- team
- account status
- accessible products
- request status
- simple rejection reason summary

일반 사용자에게 숨길 정보:

- raw role claims
- internal policy IDs
- auth provider debug values
- full approval audit
- admin notes
- database IDs
- secret, token, private URL

---

## 10. Permission Status Component

Permission status component는 account page, no access screen, product login 후 gate screen에서 재사용한다.

기본 구성:

```text
Status badge
Product name
Access level
Request state
Updated time
Next action
```

상태 모델:

| State | Badge | User Meaning | Primary Action |
|---|---|---|---|
| `approved` | success | 사용 가능 | 제품으로 이동 |
| `pending` | info/amber | 검토 중 | 신청 상태 보기 |
| `rejected` | critical/amber | 승인되지 않음 | 재요청 또는 문의 |
| `not_requested` | neutral | 아직 요청하지 않음 | 권한 요청 |
| `insufficient_permission` | amber/critical | 현재 권한 부족 | 추가 권한 요청 |
| `suspended` | critical | 사용 제한 | 관리자 문의 |

표시 원칙:

- status badge와 text를 함께 사용한다.
- 접근 가능 제품과 접근 불가 제품을 시각적으로 섞지 않는다.
- disabled product card에는 reason이 있어야 한다.
- "바로 사용 가능"처럼 보이는 copy는 approved 상태에서만 쓴다.

---

## 11. Request Status Component

Request status component는 이용 신청 완료, 권한 추가 요청, rejected 안내에서 사용한다.

기본 구성:

```text
Request title
Current status badge
Requested product(s)
Submitted time
Reviewer note summary if available
Expected next step
Safe action
```

상태 모델:

| State | UI Direction |
|---|---|
| `submitted` | 접수 완료, 검토 예정 |
| `pending_review` | 검토 중, 소속/팀 확인 중 |
| `need_more_info` | 추가 정보 필요, 수정/문의 CTA |
| `approved` | 승인 완료, 접근 가능 제품 표시 |
| `rejected` | 승인되지 않음, 사유 summary와 재요청 |
| `cancelled` | 요청 취소됨, 새 요청 가능 |

좋은 상태:

- pending 상태가 사용 가능처럼 보이지 않는다.
- rejected 상태는 차분하지만 명확하다.
- reviewer internal note를 그대로 노출하지 않는다.
- next action이 하나로 좁혀져 있다.

---

## 12. No Access / Insufficient Permission

No access 화면의 목적은 사용자를 막는 것이 아니라, 왜 막혔고 무엇을 요청해야 하는지 알려주는 것이다.

권장 구조:

```text
Status badge
Short reason
Requested product
Current account summary
Available products if any
Request access CTA
Back to available product
Help link
```

Copy 예시:

- `현재 계정으로는 AdMate Lens에 접근할 수 없습니다.`
- `Lens 권한을 요청하거나 접근 가능한 제품으로 이동해 주세요.`
- `요청이 접수되면 검토 상태를 계정 페이지에서 확인할 수 있습니다.`

금지:

- "권한 없음" 한 줄만 표시
- 내부 role/debug 값 표시
- 사용자가 승인 없이 바로 사용할 수 있을 것처럼 보이는 CTA
- `Openclaw 권한이 없습니다` 같은 내부 engine 중심 문구

---

## 13. Error / Empty / Loading States

| State | Direction |
|---|---|
| Loading account | skeleton, `계정 정보를 확인하고 있습니다` |
| Loading permissions | product list skeleton, action disabled |
| Empty access | no access reason, request CTA |
| Empty request history | `아직 요청 내역이 없습니다`, request CTA |
| Auth error | 짧은 error, retry, help link |
| Network error | retry 가능 여부와 support path |
| Permission stale | updated time과 refresh CTA |
| Session expired | 다시 로그인 CTA |

원칙:

- error가 내부 시스템 blame으로 보이지 않게 한다.
- loading 중 form layout이 크게 흔들리지 않게 한다.
- stale permission은 approved처럼 보이지 않게 한다.
- mobile에서 error text가 input/button을 밀어내거나 겹치지 않게 한다.

---

## 14. Copy Guideline

톤:

- 차분하고 명확한 업무용 문장
- 짧은 한국어 동사형 CTA
- 사용자의 next action이 보이는 안내
- 승인/검토/거절 상태를 애매하게 숨기지 않음

권장 표현:

- `AdMate 로그인`
- `AdMate Compass 로그인`
- `이용 신청`
- `권한 요청`
- `검토 중`
- `승인 완료`
- `추가 정보 필요`
- `접근 가능한 제품`
- `현재 계정으로 접근할 수 없습니다`
- `계정 페이지에서 요청 상태를 확인할 수 있습니다`

CTA 예시:

- `로그인`
- `비밀번호 설정`
- `재설정 메일 받기`
- `이용 신청`
- `권한 요청`
- `상태 확인`
- `접근 가능한 제품 보기`
- `관리자에게 문의`

---

## 15. Forbidden Copy

사용자-facing 화면에서 금지:

- `Openclaw 로그인`
- `Hermes 권한 승인`
- `Hermes 관리자에게 요청`
- `Agent memory 권한`
- `service role`
- `schema 권한`
- `auth provider debug`
- `승인 없이 바로 사용 가능합니다`
- `신청 즉시 모든 제품을 사용할 수 있습니다`
- `관리자 권한으로 계속하기`
- `내부 콘솔로 이동`

주의:

- `Agent Core 관리자 로그인`은 내부 관리자 콘솔 맥락에서만 제한적으로 사용한다.
- Openclaw/Hermes는 일반 사용자에게 제품명이 아니라 내부 engine이다.
- 승인 전 상태는 `pending`, `검토 중`, `접수 완료`처럼 표현한다.

---

## 16. Mobile / Desktop QA

Desktop:

- login card와 context block이 균형 있게 보인다.
- 제품별 variant가 다른 제품처럼 튀지 않는다.
- profile/account page에서 accessible products와 request status가 한 화면에서 스캔된다.
- no access screen이 명확한 reason과 request CTA를 가진다.

Mobile:

- input, helper, error, CTA가 겹치지 않는다.
- keyboard가 올라와도 primary action이 도달 가능하다.
- permission status list가 card height를 과도하게 늘리지 않는다.
- pending/rejected reason text는 clamp/detail 전략을 가진다.
- product chips/badges가 긴 한국어 문장과 충돌하지 않는다.

---

## 17. Product Agent Prompt Seed

제품 Agent에게 전달할 때는 아래 seed를 제품별로 조정한다.

```text
Follow AdMate Auth / Account UX Direction v1.
Implement only approved UI surfaces for login, password setup/reset, profile/account, permission status, request status, and no access states.
Do not modify auth logic, API contracts, DB schema, permissions calculation, session behavior, environment variables, email delivery, tokens, or secrets.
Use AdMate Operational Intelligence Console: neutral background, white surfaces, #E5E5E5 borders, compact typography, 8px or smaller radius, and only a thin product accent.
Use product-specific login titles such as "AdMate Compass 로그인" or "AdMate Lens 로그인".
Do not use "Openclaw 로그인" or expose Hermes/Openclaw internals to general users.
Keep Homepage as the primary 이용 신청 entry.
Show general users only organization, team, account status, accessible products, and request state.
Keep detailed audit/authority views inside Agent Core / Sentinel admin console.
Verify desktop/mobile layout, Korean text stability, focus states, pending/approved/rejected/no access states, and no internal link exposure.
```

---

## 18. Next Gate Candidate

권장 다음 Gate:

```text
Gate Design-Auth-Account-Implementation-1
```

예상 입력:

- 각 제품의 현재 login/account screenshots
- 현재 auth routes 목록
- 현재 account/profile surface 목록
- 권한 상태 sample with sensitive values removed
- mobile screenshots

예상 산출물:

- 제품별 UI-only implementation plan
- no-touch auth/API/DB/env confirmation
- login variant screenshot QA
- permission status component QA
- request status copy QA
- no access state QA
