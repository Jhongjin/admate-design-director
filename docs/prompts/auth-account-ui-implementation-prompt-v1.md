# Auth / Account UI Implementation Prompt v1

작성일: 2026-05-07
Gate: Design-Director-4
대상: AdMate 제품 Agent 공통
범위: 향후 제품 repo에서 승인된 UI-only implementation Gate가 열렸을 때 전달할 실행 프롬프트. 이 문서 자체는 구현을 실행하지 않는다.

---

## 1. Purpose

이 프롬프트는 AdMate 전체 제품의 로그인, 이용 신청, 비밀번호 설정/재설정, profile/account, permission status, request status, no access 화면을 공통 UX 기준으로 정리하기 위한 제품 Agent 전달용 문안이다.

이 Gate에서는 제품 repo 코드 수정, 실제 auth 구현, API/DB/env 변경을 하지 않는다.

---

## 2. Product Agent Execution Prompt

아래 프롬프트를 제품 Agent에게 전달한다.

```text
You are working on an approved AdMate Auth / Account UI-only implementation Gate.

Goal:
Align login, request access, password setup/reset, profile/account, permission status, request status, and no access screens with AdMate Auth / Account UX Direction v1.

Read first:
- AGENTS.md
- README.md
- existing auth/account UI files
- existing route structure for login, account/profile, password setup/reset, no access
- existing product design direction document
- existing design tokens/components

Important:
Start with read-only inspection and report current routes, surfaces, files, and risk.
Do not implement until the current Gate explicitly approves UI changes.

Hard constraints:
- Do not modify auth logic.
- Do not modify API contracts.
- Do not modify DB schema.
- Do not modify permission calculation.
- Do not modify session behavior.
- Do not modify environment variables.
- Do not modify email delivery or password reset token behavior.
- Do not modify invite token behavior.
- Do not add new auth providers.
- Do not add image/video/assets.
- Do not expose secrets, tokens, credentials, private URLs, real account data, campaign names, advertiser names, or raw campaign-level data.
- Do not edit copied reference documents.

Allowed UI-only scope after explicit approval:
- layout
- spacing
- typography
- product-specific login title and subtitle
- form label/helper/error text
- button hierarchy
- status badge treatment
- permission status component
- request status component
- no access / insufficient permission screen
- profile/account information architecture
- loading, empty, error states
- keyboard/focus/accessibility states
- mobile responsiveness

Design direction:
- Use AdMate Operational Intelligence Console.
- Use neutral app background, white surfaces, #E5E5E5 borders, compact typography, and radius of 8px or less.
- Keep product accent thin and muted.
- Keep Homepage as the primary 이용 신청 entry.
- Product login pages should preserve context:
  - AdMate Compass 로그인
  - AdMate Lens 로그인
  - AdMate Sentinel 로그인
  - AdMate Foresight 로그인
  - AdMate 로그인 for generic account entry
- Do not show "Openclaw 로그인" to general users.
- Do not overexpose Hermes or Openclaw authority details to general users.
- Show internal audit and authority details only inside Agent Core / Sentinel admin console.

Required surfaces:
1. Homepage request access entry
2. Product login screen
3. Password setup/reset guidance screen
4. Profile/account page
5. Permission status component
6. Request status component
7. Pending/approved/rejected states
8. No access / insufficient permission screen
9. Loading, empty, error states

General user account page must show:
- name or email
- organization
- team
- account status
- accessible products
- product permission state
- pending/rejected request state
- request additional platform action

General user account page must not show:
- raw role claims
- internal policy IDs
- database IDs
- auth provider debug values
- full approval audit
- admin notes
- secrets, tokens, credentials, private URLs

Permission status component:
- approved: success badge, product link enabled
- pending: info/amber badge, status details
- rejected: amber/critical badge, reason summary and request action
- not_requested: neutral badge, request action
- insufficient_permission: amber/critical badge, request additional access
- suspended: critical badge, contact admin/help

Request status component:
- submitted
- pending_review
- need_more_info
- approved
- rejected
- cancelled

Copy rules:
- Use short Korean labels and action verbs.
- Use "이용 신청", "권한 요청", "검토 중", "승인 완료", "접근 가능한 제품".
- Avoid making pending access look approved.
- Avoid "승인 없이 바로 사용 가능합니다".
- Avoid "Openclaw 로그인".
- Avoid exposing Hermes permissions to general users.

Desktop QA:
- Login card and product context are balanced.
- Product variants share one structure.
- Profile/account page is scannable.
- Permission and request status are visually distinct.

Mobile QA:
- Inputs, helper text, errors, and actions do not overlap.
- Keyboard/focus flow remains usable.
- Permission cards wrap safely.
- Pending/rejected reason text has clamp/detail behavior.
- Product badges and Korean labels do not collide.

Verification:
- Run the product repo's existing type-check/build/test commands if this becomes an implementation Gate.
- Run visual QA for desktop and mobile.
- Confirm auth/API/DB/env/session behavior is unchanged.
- Confirm no internal admin links are exposed to public or general-user surfaces.
- Confirm no secret values or real account/campaign/client data are introduced.

Report format:
Gate:
Product:
Repo/path:
Branch:
Current routes:
Current auth/account surfaces:
Files proposed for UI-only change:
No-touch files/areas:
Implemented UI surfaces:
Desktop QA:
Mobile QA:
Permission states covered:
Request states covered:
Forbidden copy scan:
Internal link exposure scan:
Commands run:
Risks:
Rollback:
```

---

## 3. Product-Specific Prompt Additions

### Compass

```text
Use "AdMate Compass 로그인".
Context copy should focus on policy and guide evidence access.
Do not change RAG, search, crawler, ingestion, embeddings, DB, API, auth, or environment.
After login, keep account/permission UI separate from answer/source UI.
```

### Lens

```text
Use "AdMate Lens 로그인".
Context copy should focus on capture requests and proof management.
Do not change capture output, rendering, injection, composite logic, platform templates, DB, API, auth, or environment.
Do not place permission UI inside capture output preview.
```

### Sentinel / Agent Core

```text
For general monitoring users, use "AdMate Sentinel 로그인".
For internal admin console only, "AdMate Agent Core 관리자 로그인" is allowed.
Do not use "Openclaw 로그인".
Do not expose Hermes/Openclaw internals to general users.
Do not change monitoring thresholds, suppression logic, Slack actions, audit logging, operator action logging, DB, API, auth, or environment.
```

### Foresight

```text
Use "AdMate Foresight 로그인".
Context copy should focus on benchmark, forecast, and planning access.
Do not expose raw campaign-level data in account/profile examples.
Do not change prediction model, data pipeline, DB, API, auth, or environment.
```

### Homepage

```text
Homepage is the primary 이용 신청 entry.
Keep request access CTA public-safe.
Do not expose internal admin routes, write actions, approval actions, private dashboards, real account data, campaign names, advertiser names, or credentials.
```

---

## 4. Review Checklist For Design Director

Before approving a product implementation Gate, confirm:

- [ ] Scope is UI-only.
- [ ] Auth/API/DB/env/session behavior is no-touch.
- [ ] Homepage remains the primary request entry.
- [ ] Product login title preserves product context.
- [ ] Product accent is thin and muted.
- [ ] General user account page hides internal authority detail.
- [ ] Agent Core / Sentinel admin console keeps detailed audit/authority views.
- [ ] Pending, approved, rejected, no access states are visually distinct.
- [ ] Mobile input and permission status layout does not overlap.
- [ ] Forbidden copy is absent.
- [ ] No image/video/asset was added.
- [ ] No secret, token, credential, real account data, campaign data, advertiser data was introduced.
