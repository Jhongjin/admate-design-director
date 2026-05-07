# AdMate User Access Roles and Approval Model v1

작성일: 2026-05-03

목적: AdMate 플랫폼을 사용하는 사람의 유형, 접근 요청, 관리자 승인, 프로젝트 권한, Hermes 학습 권한, 감사 로그 원칙을 하나의 운영 기준으로 정리한다. 이 문서는 `openclaw-monitor`의 `/access-request`, `/users/access-requests`, `openclaw.users`, `command_center_project_members`, Hermes learning authority 설계의 기준 문서다.

---

## 1. 최상위 원칙

AdMate는 사내 광고 운영 자동화 플랫폼이다. 따라서 접근 권한은 공개 self-signup이 아니라 **접근 요청 -> 관리자 승인 -> 권한 부여 -> 감사 기록** 흐름으로 운영한다.

절대 원칙:

```text
1. 신규 사용자는 기본적으로 최소 권한에서 시작한다.
2. 사용자가 직접 role을 확정하지 않는다.
3. Hermes 학습 권한은 명시적으로 승인된 사용자에게만 부여한다.
4. 권한 변경, 승인, 반려, 학습 권한 변경은 audit/operator 기록을 남긴다.
5. secret, token, service role key, env 값은 화면/로그/문서에 출력하지 않는다.
```

---

## 2. 사용자 유형

AdMate 사용자는 단순히 "관리자/사용자"로 나누지 않는다. 시스템 권한, 프로젝트 권한, Hermes 학습 권한을 분리해서 관리한다.

| 유형 | 권장 시스템명 | 의미 |
|---|---|---|
| 슈퍼관리자 | `super_admin` | 전체 플랫폼, 권한, 학습, 보안, 운영, 배포 방향을 통제한다. 현재 프로젝트 총괄자 역할이다. |
| 프로젝트 관리자 | `admin` 또는 `project_admin` | 특정 프로젝트/제품에 대한 접근 권한과 운영 통제권을 가진다. 사용자라기보다 프로젝트 운영/개발 관리자에 가깝다. |
| 학습 영향 사용자 | `trusted_user` 또는 `reviewer` | Hermes 학습 데이터에 영향을 줄 수 있는 사용자다. 피드백이 학습 후보 또는 검토 흐름에 반영될 수 있다. |
| 일반 사용자 | `user` | 플랫폼 기능은 사용하지만 Hermes 학습에는 기본적으로 영향을 주지 않는다. |
| 조회 사용자 | `viewer` | 모든 플랫폼 또는 지정 플랫폼에 접근 가능하지만 조회만 가능하다. |

---

## 3. 권한 3축 모델

권한은 하나의 role 값으로만 관리하지 않는다. 다음 3축을 분리한다.

### 3.1 System Role

시스템 전체 수준의 권한이다.

| 값 | 의미 |
|---|---|
| `super_admin` | 전체 관리, 권한 부여, 학습 반영, 시스템 운영 통제 |
| `admin` | 관리 범위 내 운영/권한 관리 |
| `user` | 일반 사용 |

기본값:

```text
system_role = user
```

### 3.2 Project Role

제품 또는 프로젝트 단위 권한이다.

| 값 | 의미 |
|---|---|
| `owner` | 해당 프로젝트의 전체 운영 책임자 |
| `editor` | 해당 프로젝트 데이터 입력/수정 가능 |
| `viewer` | 해당 프로젝트 조회만 가능 |

Command Center 기준:

```text
owner/editor = 본인 프로젝트 weekly update 입력 가능
viewer = 조회만 가능
admin/super_admin = 관리 범위 또는 전체 프로젝트 수정 가능
```

### 3.3 Hermes Learning Authority

Hermes 학습에 영향을 줄 수 있는 권한이다.

| 필드 | 값 | 의미 |
|---|---|---|
| `can_train_hermes` | `true/false` | 사용자 피드백이 Hermes 학습 후보에 들어갈 수 있는지 |
| `hermes_reviewer_enabled` | `true/false` | 학습 후보 검토자 역할을 할 수 있는지 |
| `learning_scope` | `all / managed_only / none` | 학습 영향 범위 |
| `feedback_weight` | number | 피드백 반영 가중치 |

신규 승인자의 기본값:

```text
can_train_hermes = false
hermes_reviewer_enabled = false
learning_scope = none
feedback_weight = 0
```

---

## 4. 접근 요청 흐름

현재 Openclaw 운영 기준:

```text
사용자
-> sentinel.admate.ai.kr/access-request
-> 접근 요청 제출
-> openclaw.access_requests pending 저장
-> 관리자 검토
-> users/access-requests 승인 또는 반려
-> 승인 시 openclaw.users profile 활성화
-> project member 권한 부여
-> operator_actions / audit_logs 기록
```

접근 요청 화면 권장 route:

```text
/access-request
```

관리자 검토 화면:

```text
/users/access-requests
```

---

## 5. 접근 요청 입력 필드

접근 요청 폼은 최소 정보만 받는다. 권한은 사용자가 확정하지 않고 관리자가 승인 시 부여한다.

필수:

| 필드 | 설명 |
|---|---|
| 이름 | 실명 또는 사내 표시명 |
| 회사 이메일 | 로그인/초대/권한 식별 기준 |
| 소속 실/팀 | 운영 책임과 승인 판단 기준 |
| 사용 목적 | 어떤 업무를 위해 접근하는지 |
| 접근 희망 플랫폼 | Compass, Sentinel, Lens, Foresight, Agent Core 등 |
| 요청 권한 수준 | 조회 / 입력·운영 / 프로젝트 관리 |

선택:

| 필드 | 설명 |
|---|---|
| 휴대폰 번호 | SMS 알림이 실제 필요한 경우에만 |
| 담당 프로젝트 | Command Center 또는 제품별 owner/editor 매핑 참고 |

---

## 6. 승인/반려 운영 원칙

승인 시:

```text
1. openclaw.users profile을 생성하거나 활성화한다.
2. 기본 system_role은 user로 시작한다.
3. 요청 목적에 맞는 project_role을 부여한다.
4. Hermes 학습 권한은 기본 false / none으로 둔다.
5. 필요한 경우에만 admin 또는 Hermes reviewer 권한을 별도 부여한다.
6. operator_actions와 audit_logs를 기록한다.
```

반려 시:

```text
1. access request 상태를 rejected로 변경한다.
2. 반려 사유를 남긴다.
3. operator_actions와 audit_logs를 기록한다.
4. 사용자 profile과 project member 권한은 만들지 않는다.
```

---

## 7. Supabase Auth 현재 한계와 다음 개선

현재 완료된 범위:

```text
접근 요청
-> 관리자 승인
-> openclaw.users 운영 profile 생성/활성화
-> command_center_project_members 등 project 권한 매핑
-> audit/operator 기록
```

현재 제외된 범위:

```text
Supabase Auth 계정 생성
초대 메일 발송
임시 비밀번호 발급
비밀번호 재설정 자동 안내
```

운영상 의미:

```text
승인된 사용자가 실제 로그인하려면 Supabase Auth 계정이 이미 있거나,
관리자가 별도로 계정을 생성/초대해야 한다.
```

다음 개선 후보:

| 방식 | 장점 | 주의 |
|---|---|---|
| 초대 메일 방식 | 비밀번호를 운영자가 직접 다루지 않아 안전함 | 메일 발송 설정과 초대 만료 정책 필요 |
| 관리자 수동 발급 | 구현이 단순함 | 운영 부담이 있고 비밀번호 취급 위험이 큼 |
| 임시 비밀번호 자동 발급 | 빠른 온보딩 가능 | secret 취급/전달 리스크가 커서 권장도가 낮음 |

권장:

```text
초대 메일 방식 우선 검토.
비밀번호를 Agent, 문서, 로그, 채팅에 노출하는 방식은 금지.
```

---

## 8. 제품별 접근 권한 예시

| 사용자 예시 | system_role | project_role | Hermes 권한 |
|---|---|---|---|
| 전체 총괄자 | `super_admin` | 전체 owner 상당 | `can_train_hermes=true`, `learning_scope=all` 가능 |
| Sentinel 관리자 | `admin` | Sentinel owner | 관리 범위 내 reviewer 가능 |
| Command Center 입력 담당자 | `user` | 담당 프로젝트 editor 또는 owner | 기본 false |
| 정책 전문가 리뷰어 | `user` | Compass editor | 필요 시 `can_train_hermes=true`, `learning_scope=managed_only` |
| 임원/참관자 | `user` | viewer | false / none |

---

## 9. 화면 접근 원칙

| 화면 | 접근 원칙 |
|---|---|
| `/login` | 공개 |
| `/access-request` | 공개 |
| `/users/access-requests` | admin/super_admin |
| `/users` | admin/super_admin 또는 권한 관리 허용 사용자 |
| `/command-center/input` | 로그인 사용자, 프로젝트 owner/editor/admin/super_admin |
| `/command-center/updates` | 운영 권한 사용자 |
| public Command Center API | homepage 서버 전용 read key 보호 |
| `home.admate.ai.kr/command-center` | 임원용 read-only 공개 또는 제한 정책에 따라 운영 |

---

## 10. Repo별 책임

| repo | 책임 |
|---|---|
| `openclaw-monitor` | 접근 요청, 승인/반려, Auth 연계, project 권한, Hermes 권한, audit/operator 기록 |
| `admate-homepage` | 공식 링크, 임원용 read-only Command Center 표시 |
| `admate-docs` | 권한 모델과 운영 원칙의 source of truth |
| `admate-capture-pro` | Lens 기능 권한의 소비자. 권한 source of truth는 Openclaw 기준을 따른다. |
| `Jhongjin-admate-guide-codex` | Compass 기능 권한의 소비자. 권한 source of truth는 Openclaw 기준을 따른다. |

---

## 11. Agent 지시 원칙

권한/가입/승인 관련 작업을 맡은 Agent는 다음을 지킨다.

```text
1. 먼저 이 문서를 읽는다.
2. 기존 login, Command Center input, public read API를 깨지 않는다.
3. 권한 변경은 최소 변경으로 진행한다.
4. secret/env/token/service role key 값은 출력하지 않는다.
5. 승인/반려/권한 변경은 audit_logs와 operator_actions 기록을 확인한다.
6. Hermes 학습 권한은 기본 false / none이다.
7. Supabase Auth 계정 자동화는 별도 승인 전 구현하지 않는다.
```

검증 기준:

```text
npm run check:secrets
npm run test:access-requests
npm run admin:check-access-requests-schema
npm run lint
npm run verify:harness
npm run build
npm run smoke:access-requests
```

---

## 12. 최종 요약

AdMate 접근 권한은 다음 구조로 운영한다.

```text
공개 가입이 아니라 접근 요청 기반이다.
관리자가 승인해야 운영 profile과 프로젝트 권한이 생긴다.
기본 사용자는 Hermes 학습에 영향을 주지 않는다.
학습 영향 권한은 별도 승인된 trusted/reviewer 사용자에게만 부여한다.
모든 승인, 반려, 권한 변경은 감사 가능해야 한다.
```

