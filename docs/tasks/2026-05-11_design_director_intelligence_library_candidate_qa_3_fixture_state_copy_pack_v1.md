# Design Director Intelligence Library Candidate QA 3 Fixture-state Copy Pack v1

Date: 2026-05-11
Gate: Design-IntelLib-Candidate-QA-3
Status: docs-only gate
Depends on:
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_1_state_matrix_v1.md`
- `docs/tasks/2026-05-11_design_director_intelligence_library_candidate_qa_2_wireframe_acceptance_checklist_v1.md`

Scope: define Korean UI copy candidates for future Intelligence Library
fixture states. This gate does not implement UI, create screenshots, generate
media, edit product repos, change API/DB/auth/env, or call production services.

## Purpose

Future Intelligence Library QA needs state copy that is clear enough for
operators and safe enough for screenshots.

The copy should make state and action obvious without leaking source internals.

## State Copy Pack

| State | Badge copy | Body copy | Primary action |
| --- | --- | --- | --- |
| `candidate` | 검토 전 후보 | 이 인사이트는 아직 승인되지 않았습니다. 근거와 사용 범위를 확인한 뒤 검토를 시작하세요. | 검토 시작 |
| `draft` | 초안 | 작성 중인 인사이트입니다. 승인 전에는 추천이나 재사용 흐름에 노출하지 않습니다. | 검토 요청 |
| `in-review` | 검토 중 | 담당자가 근거, 표현, 사용 범위, 안전 기준을 확인하고 있습니다. | 검토 상태 보기 |
| `approved` | 승인됨 | 정의된 범위 안에서 재사용할 수 있습니다. 사용 전 근거와 최신성을 함께 확인하세요. | 재사용 |
| `needs-update` | 업데이트 필요 | 근거가 오래되었거나 일부 기준이 바뀌었습니다. 재사용 전 내용을 갱신해야 합니다. | 업데이트 요청 |
| `deprecated` | 사용 중단 | 새 작업에는 사용하지 않는 항목입니다. 대체 항목이나 최신 기준을 확인하세요. | 대체 항목 보기 |
| `blocked` | 차단됨 | 근거 부족, 안전 문제, 또는 비공개 정보 위험으로 사용할 수 없습니다. | 조치 확인 |

## Detail Panel Copy

### Source Basis

```text
근거 상태
검토 가능한 근거가 연결되어 있습니다.
```

```text
근거 부족
재사용하기에 충분한 근거가 없습니다.
```

```text
근거 확인 필요
출처 범위와 최신성을 확인해야 합니다.
```

### Scope

```text
사용 가능 범위
이 인사이트는 지정된 제품, 채널, 플랫폼, 또는 업무 흐름 안에서만 재사용할 수 있습니다.
```

```text
범위 미정
재사용 가능한 제품이나 채널이 아직 정해지지 않았습니다.
```

### Freshness

```text
최신성 확인됨
최근 검토 기준에 맞는 항목입니다.
```

```text
재검토 필요
최신 정책이나 운영 기준과 맞는지 다시 확인해야 합니다.
```

## Blocking Reasons

Use sanitized blocking reasons only:

- 근거가 충분하지 않습니다.
- 사용 범위가 정해지지 않았습니다.
- 최신성 확인이 필요합니다.
- 비공개 정보가 포함될 가능성이 있습니다.
- 검토자가 안전 기준을 확인해야 합니다.
- 대체 항목이 필요합니다.

Avoid:

- raw source payload references
- table, job, crawler, embedding, or vector names
- private customer, account, campaign, billing, credential, or provider details
- exact internal identifiers

## Empty States

| Empty state | Copy |
| --- | --- |
| No candidates | 검토할 후보가 없습니다. 새 근거가 들어오면 이곳에 표시됩니다. |
| Filter no results | 선택한 조건에 맞는 인사이트가 없습니다. 상태나 범위를 조정해 보세요. |
| Source loading | 근거 정보를 불러오는 중입니다. |
| Source unavailable | 근거 정보를 표시할 수 없습니다. 잠시 후 다시 확인하세요. |
| Blocked-only view | 차단된 항목만 표시 중입니다. 각 항목의 조치 기준을 확인하세요. |

## Action Feedback

| Action | Success copy | Failure copy |
| --- | --- | --- |
| Approve | 승인되었습니다. 지정된 범위에서 재사용할 수 있습니다. | 승인할 수 없습니다. 근거와 안전 기준을 다시 확인하세요. |
| Request update | 업데이트 요청을 남겼습니다. | 업데이트 요청을 저장하지 못했습니다. 다시 시도하세요. |
| Deprecate | 사용 중단 상태로 변경했습니다. | 사용 중단 처리에 실패했습니다. 다시 시도하세요. |
| Block | 차단 상태로 변경했습니다. | 차단 처리에 실패했습니다. 안전 기준을 다시 확인하세요. |

## Screenshot Safety

QA screenshots may show this copy only with synthetic fixture data. Screenshots
must not include private source details, raw identifiers, credentials, provider
payloads, or exact customer context.

## No-Touch Confirmation

This gate does not perform:

- product repo edits
- UI implementation
- API or DB changes
- auth, env, or storage changes
- screenshot capture
- generated media or asset creation
- production traffic
- real customer data review
- secret, env, token, cookie, session, credential, signed URL, raw provider, or
  raw source output

## Next Gate

Recommended next gate:

```text
Gate Design-IntelLib-Candidate-QA-4 product handoff note
```

That gate should package the state matrix, wireframe checklist, and copy pack
into a short product-team handoff.
