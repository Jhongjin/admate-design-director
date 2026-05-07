# Compass UI Production QA Prompt v1

작성일: 2026-05-07
Gate: Design-Director-3
대상: AdMate Compass 제품 Agent
범위: production-facing UI QA 실행 프롬프트. 이 문서는 제품 repo 코드 수정을 지시하지 않는다.

---

## 1. Purpose

이 문서는 Design-Director-2의 `compass-ui-review-prompt-v1.md`를 실제 `admate-compass` Agent에게 바로 전달 가능한 production QA 프롬프트로 구체화한다.

목표:

```text
Compass production UI에서 source/evidence가 답변 신뢰의 중심인지 확인한다.
Read-only QA를 먼저 수행하고, 코드 수정은 다음 Gate로 분리한다.
```

Compass는 정책을 답하는 제품이지만, 사용자가 신뢰해야 하는 것은 "그럴듯한 답변"이 아니라 "검증 가능한 source와 evidence"다. 따라서 QA는 answer polish보다 source/evidence, noDataFound, generation-limited, long Korean text stability를 우선한다.

---

## 2. QA 대상 화면

우선 QA 대상:

| Target | QA Focus |
|---|---|
| `/` | 첫 화면이 Compass의 정책/RAG 목적을 명확히 보여주는가 |
| `/chat-ollama` | production-facing chat flow가 evidence-first로 보이는가 |
| Answer card | 답변 요약, risk/confidence, generation state가 과장 없이 보이는가 |
| Source/evidence panel | verified source, source title, original title, availability가 추적 가능한가 |
| noDataFound state | vendor-neutral하고 불필요한 환각을 유도하지 않는가 |
| generation-limited state | Ollama failure 또는 generation 제한 상태에서도 verified sources가 보존되는가 |
| Mobile layout | source panel이 채팅을 밀어내거나 덮지 않고 긴 한국어 텍스트가 안정적인가 |

보조 QA 대상:

- question input
- platform/policy domain context
- source drawer or detail panel
- loading/searching state
- error/partial success state
- keyboard/focus path
- HTML text content when screenshots are unavailable

---

## 3. QA 핵심 기준

### 3.1 Evidence First

확인할 것:

- source/evidence가 answer보다 시각적으로 뒤로 밀리지 않는다.
- verified source와 limited evidence가 badge와 설명으로 구분된다.
- source panel이 광고성 카드, marketing card, decorative recommendation처럼 보이지 않는다.
- source evidence와 answer card가 서로 연결되어 보인다.
- source 없는 답변이 높은 confidence처럼 보이지 않는다.

통과 기준:

```text
사용자가 답변을 읽기 전에, 또는 답변과 동시에,
어떤 source를 근거로 삼았는지 확인할 수 있다.
```

### 3.2 Internal Term Mapping

내부 용어는 사용자-facing copy로 매핑되어야 한다.

| Internal Term | User-Facing Direction |
|---|---|
| `retrievalMethod` | 검색 기준, 근거 검색 방식, source 탐색 방식 |
| `sourceQuality` | 출처 신뢰도, 근거 품질, 검증 수준 |
| `sourcesCount` | 확인된 출처 수, 참고한 source 수 |
| `schema=compass` | 사용자에게 직접 노출하지 않음 |
| `noDataFound` | 관련 근거를 찾지 못함, 검색 범위 밖일 수 있음 |
| `generation-limited` | 답변 생성 제한, 근거는 확인 가능 |

나쁜 상태:

- `retrievalMethod`, `sourceQuality`, `schema`, raw `sourcesCount` 같은 내부 field name이 그대로 사용자 UI에 보인다.
- 사용자가 이해하기 어려운 debug phrase가 answer card나 source card에 노출된다.

### 3.3 Korean Text Stability

확인할 것:

- 긴 한국어 정책 답변이 answer card, source card, mobile panel 밖으로 넘치지 않는다.
- 한국어, 영어 플랫폼명, 숫자, URL이 섞인 문장이 badge/button과 겹치지 않는다.
- source excerpt는 line-clamp 또는 detail expansion 전략을 가진다.
- URL은 hostname 우선 표시 또는 안정적인 wrapping/copy 전략을 가진다.
- streaming-like rendering이 있다면 card height jump가 과하지 않다.

Pretext 판단:

- Pretext.js는 production dependency로 바로 추가하지 않는다.
- QA 결과 CSS-only line-clamp/wrapping으로 충분한지 먼저 판단한다.
- 필요하면 다음 Gate에서 Compass long-text PoC로 분리한다.

### 3.4 noDataFound State

확인할 것:

- noDataFound는 특정 vendor나 platform 탓으로 단정하지 않는다.
- 근거가 없는데도 답변을 추측하도록 유도하지 않는다.
- "근거를 찾지 못함", "질문 범위를 좁혀 다시 검색", "다른 platform/domain 선택" 같은 안전한 next action을 제공한다.
- noDataFound와 generation failure가 같은 error처럼 보이지 않는다.

금지 copy:

```text
정책상 문제 없습니다.
아마 허용됩니다.
Google/Meta/Naver 쪽 문제가 있습니다.
Compass가 확인했으므로 안전합니다.
```

### 3.5 Ollama Failure / Generation-Limited

확인할 것:

- Ollama failure 또는 generation-limited 상태에서도 verified sources는 사라지지 않는다.
- source search success와 answer generation failure가 분리되어 보인다.
- "답변 생성은 제한되었지만 확인된 source는 아래에서 볼 수 있음"처럼 사용자가 다음 행동을 알 수 있다.
- API/network/model failure가 source absence처럼 오해되지 않는다.

통과 기준:

```text
Generation이 실패해도 source/evidence panel은 가능한 한 보존된다.
사용자는 답변 실패와 근거 검색 결과를 분리해서 이해한다.
```

### 3.6 Platform Tone Consistency

확인할 것:

- Compass가 Homepage/Command Center와 같은 AdMate neutral system 안에 있다.
- background, surface, border, typography, badge density가 `AdMate Operational Intelligence Console` 기준을 따른다.
- Compass accent는 indigo-blue 계열로 제한적으로 쓰인다.
- dark/glass/gradient가 source/evidence 판단보다 강하지 않다.
- marketing hero나 oversized card grid가 production chat workspace를 지배하지 않는다.

---

## 4. 좋은 상태 / 나쁜 상태 예시

### 4.1 좋은 상태

Evidence-first:

- answer summary 옆에 verified source count와 source freshness가 보인다.
- source card에는 title, original title, platform, availability, collected time, excerpt가 있다.
- limited evidence는 amber badge와 짧은 설명으로 표시된다.
- noDataFound는 vendor-neutral하고 재질문/범위 조정 action을 제안한다.
- generation-limited 상태에서도 source panel은 보존된다.

Compact operational console:

- `#F7F7F7` 계열 background, white surface, `#E5E5E5` border 중심이다.
- card radius는 8px 이하이고, shadow보다 border/spacing으로 계층을 만든다.
- body text는 13-14px 수준의 업무 UI density를 유지한다.
- source/evidence가 decorative card보다 table/list에 가까운 정보 구조를 가진다.

Source identity:

- source title과 original title이 구분된다.
- source availability가 `available`, `stale`, `unavailable`, `partial` 같은 사용자-facing 상태로 표시된다.
- original URL 또는 hostname을 확인할 수 있다.
- source가 answer confidence와 연결되어 보인다.

Mobile:

- source panel은 accordion, drawer, 또는 below-answer section으로 안정적으로 분리된다.
- source panel이 chat input을 덮지 않는다.
- 긴 source excerpt는 clamp 후 detail에서 확장된다.
- button/badge text가 겹치지 않는다.

### 4.2 나쁜 상태

Evidence buried:

- answer card가 크고 source는 footer처럼 작게 붙어 있다.
- source count만 있고 source title/original title/availability가 없다.
- source card가 광고성 추천 카드나 product marketing card처럼 보인다.
- source 없는 답변이 높은 confidence처럼 보인다.

Internal leakage:

- `retrievalMethod`, `sourceQuality`, `schema=compass`, raw debug field가 사용자 UI에 그대로 노출된다.
- raw JSON, DB field, debug stack이 answer/source 영역에 보인다.

Unsafe noDataFound:

- noDataFound인데 정책 판단을 추측한다.
- vendor를 탓하거나 특정 platform 문제로 단정한다.
- "안전함" 또는 "허용됨"을 source 없이 암시한다.

Broken generation-limited:

- Ollama failure 후 verified source까지 사라진다.
- generation failure가 noDataFound처럼 보인다.
- API failure copy가 사용자의 정책 판단을 흐린다.

Visual drift:

- cinematic dark, glassmorphism, purple/blue gradient, blob/orb background가 중심이 된다.
- chat screen이 generic AI SaaS landing처럼 보인다.
- source/evidence보다 hero copy나 decorative card가 먼저 보인다.

Mobile breakage:

- source panel이 chat input을 덮는다.
- source drawer가 화면 전체를 차지해 answer와 source 연결이 끊긴다.
- 긴 한국어/URL이 card 밖으로 넘치거나 badge와 겹친다.

---

## 5. Read-Only QA 실행 프롬프트

아래 프롬프트를 `admate-compass` Agent에게 그대로 전달한다.

```text
You are the AdMate Compass product Agent.

Gate:
Design-Compass-Evidence-Production-QA-1

Goal:
Run read-only production-facing UI QA for AdMate Compass.
Focus on evidence-first RAG UX, source/evidence preservation, noDataFound safety, generation-limited behavior, and mobile Korean text stability.

Read first:
- AGENTS.md
- README.md
- package.json
- the current / and /chat-ollama UI implementation files
- the current /api/chat-ollama route implementation only for understanding response shape

Do not modify files in this Gate.
If you find issues, report them as QA findings and propose a next implementation Gate.

QA targets:
- /
- /chat-ollama
- answer card
- source/evidence panel
- noDataFound state
- generation-limited or Ollama failure state
- mobile layout
- source card title/original title/source availability display

Hard no-touch areas:
- Do not modify RAGSearchService.
- Do not modify /api/chat-ollama behavior.
- Do not modify DB schema, imports, seed data, crawler, ingestion, reembedding, reranking, embeddings, source ranking, confidence calculation, auth, environment, or secrets.
- Do not modify package dependencies.
- Do not call external APIs except the existing app route checks explicitly required for local/production QA.
- Do not expose real advertiser names, campaign names, account information, raw campaign-level data, tokens, credentials, or private URLs.

Design criteria:
- Follow AdMate Operational Intelligence Console.
- Source/evidence must be the center of trust, not a decorative footer.
- Map internal terms such as retrievalMethod and sourceQuality into user-facing Korean copy.
- Preserve verified sources when answer generation is limited or Ollama fails.
- Separate noDataFound from generation failure.
- Keep noDataFound vendor-neutral and avoid hallucination-inducing copy.
- Avoid dark/glass/gradient-heavy visual style.
- Keep Compass visually aligned with AdMate Homepage/Command Center neutral operational tone.

Required checks:
1. Inspect / and /chat-ollama visually or via screenshot if available.
2. Verify answer card hierarchy: answer, evidence, confidence/risk, follow-up action.
3. Verify source/evidence panel: source title, original title, availability, freshness, excerpt, URL/hostname.
4. Verify noDataFound state: safe copy, no vendor blame, no policy speculation.
5. Verify generation-limited/Ollama failure state: verified sources remain visible if retrieval succeeded.
6. Verify long Korean policy answer and source excerpt behavior on desktop and mobile widths.
7. Verify internal field names are not leaked into user-facing UI.
8. Verify production /chat-ollama returns HTTP 200.
9. Verify production /api/chat-ollama keeps schema=compass and sourcesCount in the response shape when sources are found.
10. Run npm run type-check, npm run build, and npm run verify:harness.

Visual QA:
- Prefer screenshots for desktop and mobile.
- If screenshots are not available, use HTML/text scan and DOM inspection to confirm user-facing copy and source fields.
- Separate visual QA evidence from API/schema checks.

Report format:
Gate:
Product:
Repo/path:
Branch:
QA date:
Routes checked:
Commands run:
Production HTTP checks:
Visual evidence:
Findings:
P0 blockers:
P1 improvements:
P2 notes:
No-touch areas confirmed:
Internal-term mapping result:
noDataFound result:
Generation-limited/Ollama failure result:
Mobile text stability result:
AdMate tone consistency result:
Recommended next Gate:

Do not commit, push, or implement fixes in this QA Gate.
```

---

## 6. 검증 체크리스트

`admate-compass` Agent가 QA 후 보고해야 하는 검증:

| Check | Expected Result |
|---|---|
| `npm run type-check` | pass |
| `npm run build` | pass |
| `npm run verify:harness` | pass or known documented non-UI blocker |
| production `/chat-ollama` HTTP | 200 |
| production `/api/chat-ollama` response shape | `schema=compass` 유지 |
| production `/api/chat-ollama` sources | sources found case에서 `sourcesCount` 유지 |
| visual QA | screenshot 또는 HTML text scan으로 분리 보고 |
| mobile QA | 긴 한국어 답변/source excerpt overflow 없음 |
| no-touch confirmation | RAGSearchService, API, DB/schema/import/reembedding/crawler 변경 없음 |

주의:

- production HTTP/API 확인은 읽기 전용 요청만 사용한다.
- secret, token, credential, private URL이 필요한 검증은 수행하지 말고 blocker로 보고한다.
- 실제 광고주명, 캠페인명, 계정정보가 포함된 payload를 쓰지 않는다.
- API response를 보고할 때 raw sensitive data를 붙여넣지 않는다.

---

## 7. HTML Text Scan 기준

스크린샷을 확보할 수 없을 때는 HTML/DOM/text scan으로 아래를 확인한다.

필수 확인:

- 사용자-facing text에 `retrievalMethod`, `sourceQuality`, `schema=compass` 같은 내부 field name이 노출되지 않는다.
- noDataFound copy가 vendor-neutral하다.
- generation-limited copy가 source preservation을 안내한다.
- source title과 original title이 둘 다 있을 경우 서로 구분된다.
- source availability가 빈 문자열이 아니라 badge/text로 표현된다.
- answer/source 영역에 raw JSON/debug dump가 기본 노출되지 않는다.

HTML text scan 결과는 screenshot QA를 대체할 수 없지만, visual QA가 제한될 때 최소 evidence로 사용할 수 있다.

---

## 8. 다음 Gate 분리 원칙

이 QA에서 issue가 발견되어도 바로 구현하지 않는다.

다음 Gate 후보:

```text
Gate Design-Compass-Evidence-Implementation-1
```

다음 Gate에서만 검토할 수 있는 작업:

- answer/source layout 조정
- user-facing copy 개선
- badge/state treatment 개선
- mobile source panel layout 조정
- Korean text wrapping/line-clamp 개선
- accessibility/focus 개선

다음 Gate에서도 계속 금지:

- RAGSearchService 변경
- `/api/chat-ollama` behavior 변경
- DB schema/import/reembedding/crawler 변경
- source ranking/confidence calculation 변경
- auth/env/secret 변경
- external API integration 변경

---

## 9. Design Director Review Output Template

Compass Agent가 QA 결과를 제출하면 Design Director는 아래 형식으로 판단한다.

```text
Gate:
Product:
Review input:
Decision:
P0 blockers:
P1 implementation candidates:
Deferred:
No-touch areas confirmed:
RAG/API/DB safety:
Source/evidence trust:
Mobile text stability:
Tone consistency:
Next Gate:
```

Decision 값:

- `Pass`
- `Pass with P1 follow-up`
- `Blocked by P0 UI trust issue`
- `Blocked by non-design risk`

P0 예시:

- verified source가 answer에서 사라짐
- noDataFound가 정책 판단을 환각처럼 유도함
- generation failure가 noDataFound처럼 보임
- mobile에서 source panel이 chat input을 덮음
- raw debug/internal fields가 사용자-facing UI에 노출됨

P1 예시:

- source title/original title hierarchy 개선 필요
- source availability badge copy 개선 필요
- long Korean excerpt clamp/detail 개선 필요
- Homepage/Command Center와 tone alignment 조정 필요

---

## 10. Reference Translation

Neuform:

- source card density, documentation UI, empty state 구조만 참고한다.
- template, brand color, gradient/glass visual은 가져오지 않는다.
- AdMate translation은 compact source/evidence panel과 neutral operational layout이다.

Pretext:

- Compass long Korean answer/source card는 PoC 후보가 될 수 있다.
- production dependency 추가는 이 Gate에서 금지한다.
- CSS-only wrapping/line-clamp가 충분한지 먼저 확인한다.

Taste direction:

- redesign/minimalist/soft reference는 RAG answer/source UI에만 제한적으로 적용 가능하다.
- 이 QA Gate에서는 설치, dependency 추가, code implementation을 하지 않는다.
