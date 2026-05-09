# Creative Studio Hyperframes Storyboard Prompt Pack v1

작성일: 2026-05-09
Gate: CreativeStudio-Hyperframes-Storyboard-1
Reference: Hyperframes reference workflow
범위: AdMate Creative Studio용 storyboard, script, scene timing, frame-spec, QA prompt pack. 영상 생성, render, image generation, 외부 code/vendor import는 하지 않는다.

---

## 1. Purpose

이 문서는 Hyperframes의 HTML-first scene/timing/frame QA 아이디어를 AdMate Creative Studio용 prompt pack으로 번역한다.

Hyperframes의 코드를 가져오지 않는다. 대신 다음 개념만 AdMate 방식으로 사용한다.

```text
Scene as structured source
Layout before animation
Timing before render
Frame QA before production
Mock safety before visual generation
```

AdMate Creative Studio에서 사용할 목표:

- executive 5min 영상의 구조화된 storyboard 작성
- media planner 90s 영상의 짧은 설명 영상 구조 작성
- frame별 message, layout, caption, disclosure, presenter cue 정의
- mock-only visual safety 검토
- 이후 영상/이미지 생성 Gate로 넘어가기 전 QA 기준 고정

---

## 2. Hard Constraints

이번 Gate와 이 prompt pack에서 금지:

- 영상 생성
- render / preview 실행
- image generation
- voiceover / TTS 생성
- transcription / background removal
- external API 호출
- Hyperframes code, package, plugin, skill, template, catalog block, asset vendor/copy
- npm/bun install 또는 build/render 실행
- product repo 코드 수정
- API, DB, auth, env, RAG, capture engine 변경
- Lens capture output 변형
- Sentinel critical operation에 motion/video-first workflow 적용
- 실제 인물 imitation
- 실제 광고주명, 캠페인명, 계정정보, 내부 dashboard screenshot, token, credential 사용

허용:

- 문서 기반 storyboard
- script block
- scene timing table
- frame-spec
- caption/disclosure/presenter cue checklist
- frame QA checklist
- mock visual safety checklist
- Creative Studio Agent에게 전달할 prompt 작성

---

## 3. Core Translation

| Hyperframes Idea | AdMate Creative Studio Translation |
|---|---|
| HTML is source of truth | Storyboard spec is source of truth |
| `data-*` timing attributes | scene id, duration, start, end, voiceover, caption metadata |
| Layout before animation | hero frame layout before motion concept |
| Composition variables | reusable product/message variables |
| Frame adapters | optional motion style notes, no runtime selection in this Gate |
| Browser preview | future screenshot/storyboard review, no render now |
| Render to MP4 | future approved production Gate only |

AdMate source of truth:

```text
One scene block = message + timing + layout + caption + disclosure + presenter cue + safety note.
```

---

## 4. Storyboard Data Model

Use this schema for each scene.

```text
scene_id:
video_type: executive_5min | planner_90s
duration_sec:
start_time:
end_time:
scene_role:
viewer_question:
message:
supporting_points:
visual_role:
layout_frame:
on_screen_text:
caption:
presenter_cue:
disclosure:
product_scope:
evidence_source:
mock_data_policy:
risk_note:
qa_status:
```

Field guidance:

| Field | Meaning |
|---|---|
| `scene_id` | Stable identifier, e.g. `exec-01`, `planner-03` |
| `scene_role` | hook, context, product map, lifecycle, proof, close |
| `viewer_question` | What the viewer should understand at this moment |
| `message` | One sentence core point |
| `visual_role` | diagram, console mock, lifecycle map, quote, metric, transition |
| `layout_frame` | static hero frame arrangement before motion |
| `on_screen_text` | text visible in frame, short Korean preferred |
| `caption` | subtitle/caption line for accessibility |
| `presenter_cue` | spoken delivery cue or presenter action |
| `disclosure` | mock-only, aggregate-only, internal-safe label if needed |
| `product_scope` | Homepage, Compass, Sentinel, Lens, Foresight, Agent Core |
| `evidence_source` | central doc or approved reference, not raw campaign data |
| `mock_data_policy` | mock, aggregate, anonymized, public-safe |
| `risk_note` | what must not be implied or shown |
| `qa_status` | draft, needs review, approved for concept, blocked |

---

## 5. Executive 5min Structure

Purpose:

```text
Explain why AdMate exists, how the product family connects, and why Agent Core creates long-term operational intelligence.
```

Audience:

- executives
- team leads
- non-technical stakeholders

Tone:

- calm, strategic, credible
- operational intelligence, not flashy AI demo
- clear business value without overclaiming implementation maturity

Total duration:

- target: 5 minutes
- recommended scene count: 9-12 scenes
- average scene duration: 20-40 seconds

Recommended scene map:

| Scene | Duration | Role | Core Message |
|---|---:|---|---|
| exec-01 | 15s | hook | 광고 운영은 반복 확인, 검수, 증빙, 학습이 끊어져 있다 |
| exec-02 | 25s | platform definition | AdMate는 광고 운영 생애주기를 연결하는 AI Agent 기반 운영 플랫폼이다 |
| exec-03 | 30s | lifecycle | 기획부터 정책 확인, 검수, 모니터링, 캡처, 학습까지 이어진다 |
| exec-04 | 30s | product map | Compass, Sentinel, Lens, Foresight가 각 단계의 전문 제품이다 |
| exec-05 | 35s | Agent Core | Openclaw/Hermes는 외부 제품이 아니라 실행/기억 공통 engine이다 |
| exec-06 | 30s | trust model | 상태, 근거, 승인, audit가 AdMate UI와 운영 구조의 중심이다 |
| exec-07 | 35s | data governance | raw campaign data와 학습 반영은 권한/승인/보안 기준으로 통제한다 |
| exec-08 | 35s | cost/ops | LLM/API 비용과 기술 변화는 운영 지표로 관리한다 |
| exec-09 | 35s | scenario | planner가 전략에 집중하도록 반복 확인과 증빙을 Agent가 맡는다 |
| exec-10 | 30s | current roadmap | 현재 제품별 성숙도와 다음 연결 지점을 차분히 제시한다 |
| exec-11 | 20s | close | AdMate는 운영 지식을 실행 가능한 자산으로 바꾼다 |

Executive visual principles:

- use abstract lifecycle maps and mock console frames
- avoid real dashboard screenshots unless explicitly approved and sanitized
- no real client/campaign/account names
- no product maturity overclaim
- Agent Core is internal layer, not standalone public product

---

## 6. Planner 90s Structure

Purpose:

```text
Show media planners that AdMate reduces repetitive work and helps them focus on strategy and judgment.
```

Audience:

- media planners
- campaign operators
- internal adoption stakeholders

Tone:

- practical
- supportive
- not replacing planners
- workflow-first

Total duration:

- target: 90 seconds
- recommended scene count: 6-8 scenes
- average scene duration: 8-15 seconds

Recommended scene map:

| Scene | Duration | Role | Core Message |
|---|---:|---|---|
| planner-01 | 8s | hook | 반복 확인과 캡처에 쓰는 시간을 줄인다 |
| planner-02 | 12s | pain points | 정책 검색, 세팅 검수, 모니터링, 증빙이 흩어져 있다 |
| planner-03 | 12s | Compass | 정책과 가이드 근거를 빠르게 확인한다 |
| planner-04 | 14s | Sentinel | 캠페인 사고를 사전 검수와 실시간 감지로 줄인다 |
| planner-05 | 12s | Lens | 캡처와 보고 증빙을 자동화한다 |
| planner-06 | 12s | Foresight | 다음 캠페인의 benchmark와 예상 성과를 준비한다 |
| planner-07 | 12s | Agent Core | 내 판단과 action이 다음 운영 기준으로 학습된다 |
| planner-08 | 8s | close | AdMate는 planner를 대체하지 않고 판단 시간을 돌려준다 |

Planner visual principles:

- show workflow sequence, not cinematic AI fantasy
- use mock task cards, status badges, source panels, capture queue illustrations
- avoid internal admin console details
- keep Korean copy short and readable
- use product accents lightly

---

## 7. Scene Block Template

Use this template for each storyboard scene.

```text
Scene:
ID:
Video type:
Duration:
Start / end:
Role:

Viewer question:

Core message:

Script:

On-screen text:

Caption:

Visual frame:
- Layout:
- Primary object:
- Secondary object:
- Product accent:
- Background/surface:

Presenter cue:

Disclosure:

Evidence/source:

Mock visual policy:

Risk note:

Frame QA:
- Text fit:
- Product boundary:
- Data safety:
- Tone:
- Accessibility:
```

Rules:

- one scene, one core message
- on-screen text should be shorter than voiceover
- presenter cue should support the message, not narrate every visual detail
- visual frame must be understandable as a static frame before any motion idea
- disclosure appears when mock, aggregate, roadmap, or conceptual visuals are shown

---

## 8. Timing Checklist

Use before approving storyboard timing.

- [ ] Total duration matches target: 5min or 90s.
- [ ] Every scene has duration and start/end time.
- [ ] Scene transitions do not hide required disclosure.
- [ ] Voiceover text fits the duration.
- [ ] Korean captions can be read at normal speed.
- [ ] No single scene carries more than one major concept.
- [ ] Product introductions are balanced and do not overrepresent one product unless intentionally scoped.
- [ ] Executive video includes business value and governance.
- [ ] Planner video includes daily workflow value and non-replacement message.
- [ ] Closing scene has one clear takeaway.

Suggested reading speed:

- Korean voiceover: keep concise; review aloud.
- Captions: prefer 1-2 short lines per beat.
- Do not force dense strategy copy into a 6-8 second scene.

---

## 9. Layout Checklist

Use before any image/video generation Gate.

- [ ] Hero frame is defined before motion.
- [ ] Primary message is visible without animation.
- [ ] Text does not overlap product mock, badge, or presenter area.
- [ ] No card-inside-card decorative composition.
- [ ] Product accent is thin and muted.
- [ ] AdMate Operational Intelligence Console tone is preserved.
- [ ] Mock console is neutral, border-first, and not a real internal screenshot.
- [ ] Source/audit/status UI is not turned into decorative background.
- [ ] Mobile/crop-safe version is considered for 16:9 and 9:16 if needed.
- [ ] Frame remains understandable when paused.

Frame format guidance:

| Format | Use | Notes |
|---|---|---|
| 16:9 | executive deck, internal presentation | primary format for 5min |
| 9:16 | short social/internal mobile teaser | only if separately requested |
| 1:1 | thumbnail or recap clip | optional |

---

## 10. Caption Checklist

- [ ] Captions are shorter than voiceover.
- [ ] Captions do not contain raw data or internal terms.
- [ ] Captions preserve product names: Compass, Sentinel, Lens, Foresight, Agent Core.
- [ ] Captions do not use Openclaw/Hermes as public-facing products.
- [ ] Captions do not say "fully automated" unless the scenario is approved.
- [ ] Captions avoid guarantee language.
- [ ] Korean text wraps cleanly in 16:9 and optional 9:16 crop.
- [ ] Important disclosure is not only spoken; it appears on screen when needed.

---

## 11. Disclosure Checklist

Use disclosure labels for mock/conceptual visuals.

Recommended labels:

- `Mock screen`
- `Concept storyboard`
- `Aggregate example`
- `Roadmap concept`
- `Illustrative workflow`
- `No real campaign data`

Korean copy options:

- `예시 화면`
- `개념 시안`
- `집계 예시`
- `로드맵 개념`
- `실제 캠페인 데이터 아님`

Disclosure required when:

- a dashboard-like UI appears
- a metric or chart appears
- a product state is not production-verified
- a roadmap feature is shown
- any internal workflow is simplified
- generated or mock visual could be confused with product UI

Disclosure must not:

- be too small to read
- disappear during the scene too quickly
- imply production readiness
- cover critical message text

---

## 12. Presenter Cue Checklist

Presenter cue is optional, but useful for executive/planner videos.

Cue fields:

```text
presenter_style:
gesture:
camera_direction:
delivery_note:
pause_point:
```

Rules:

- Do not imitate real employees or executives.
- Use generic presenter descriptions only.
- Do not reference private office, internal screens, client names, or account information.
- Presenter should not cover key text or product mock.
- Presenter is secondary to message clarity.
- For planner video, presenter tone should be helpful, not salesy.
- For executive video, presenter tone should be concise and strategic.

Example:

```text
presenter_style: generic professional presenter
gesture: small handoff gesture toward lifecycle map
camera_direction: medium shot, neutral background
delivery_note: calm, confident, no hype
pause_point: after "운영 지식 자산"
```

---

## 13. Frame QA Checklist

Use for every scene before visual production.

Content:

- [ ] One core message.
- [ ] Product role is accurate.
- [ ] Agent Core is internal common layer.
- [ ] Openclaw/Hermes are not public-facing standalone products.
- [ ] No overclaim about automation, prediction, or production maturity.

Text:

- [ ] Korean text fits.
- [ ] Captions are readable.
- [ ] No negative letter spacing.
- [ ] No viewport-based font scaling instruction.
- [ ] Long words, product names, and numbers do not collide.

Visual:

- [ ] Static hero frame works without animation.
- [ ] Status/source/audit elements are meaningful, not decorative clutter.
- [ ] Product accents are muted.
- [ ] No cinematic dark/glass/gradient-heavy style unless separately approved for a concept frame.
- [ ] No generated UI that looks like real production evidence.

Safety:

- [ ] No real campaign names.
- [ ] No advertiser/client names.
- [ ] No account information.
- [ ] No internal dashboard screenshot.
- [ ] No token, credential, env, private URL.
- [ ] No real person imitation.
- [ ] No raw campaign-level data.

Boundary:

- [ ] Lens capture output is not altered or represented as generated proof.
- [ ] Sentinel incident/approval UI is not animated as decorative content.
- [ ] Compass source/evidence is not shown as hallucinated proof.
- [ ] Foresight metrics are mock/aggregate only.

---

## 14. Mock Visual Safety Checklist

Before asking any future agent to create image/video/frames, confirm:

- [ ] Visual uses mock-only data.
- [ ] Any dashboard is labeled mock/concept.
- [ ] No private brand/client/campaign identifiers.
- [ ] No screenshot from internal systems.
- [ ] No employee likeness or real person imitation.
- [ ] No office/private workspace reproduction.
- [ ] No secret-looking strings.
- [ ] No generated Lens capture result replacing real capture output.
- [ ] No Sentinel alert action shown as animated entertainment.
- [ ] No unsupported product maturity claim.
- [ ] Asset storage/review location is defined before creation.
- [ ] Human review is required before product repo inclusion.

Default:

```text
If safety is unclear, do not generate visual assets.
Write a safer text storyboard instead.
```

---

## 15. Executive 5min Prompt

Use this prompt to ask a Creative Studio Agent for a text-only storyboard.

```text
You are preparing a text-only storyboard for an AdMate executive 5-minute video.

Use the Hyperframes reference only as a workflow idea:
- scene as structured source
- timing table
- layout before animation
- frame QA before production

Do not use Hyperframes code, packages, skills, templates, catalog blocks, assets, or commands.
Do not generate video, images, voiceover, audio, captions as files, or rendered frames.
Do not call external APIs.
Do not modify product repos.

Goal:
Create a 5-minute executive storyboard explaining AdMate as an AI Agent-based advertising operations platform.

Must cover:
- why AdMate exists
- campaign lifecycle
- Compass, Sentinel, Lens, Foresight roles
- Agent Core as internal execution/memory layer
- status, evidence, approval, audit as trust model
- data governance and cost awareness
- roadmap without overclaiming

Tone:
calm, strategic, credible, operational.

Output:
1. 9-12 scene timing table
2. Scene block for each scene
3. On-screen text and captions
4. Presenter cue per scene
5. Disclosure labels where needed
6. Frame QA checklist result
7. Mock visual safety checklist result

Hard constraints:
- no real campaign, advertiser, account, employee, dashboard, token, credential, private URL, or raw data
- no real person imitation
- no internal screenshot
- no Lens capture output transformation
- no Sentinel critical operation animation
- no product maturity overclaim
- label mock/concept/aggregate visuals clearly
```

---

## 16. Planner 90s Prompt

Use this prompt to ask a Creative Studio Agent for a text-only storyboard.

```text
You are preparing a text-only storyboard for an AdMate 90-second media planner intro video.

Use the Hyperframes reference only as a workflow idea:
- scene as structured source
- timing table
- layout before animation
- frame QA before production

Do not use Hyperframes code, packages, skills, templates, catalog blocks, assets, or commands.
Do not generate video, images, voiceover, audio, captions as files, or rendered frames.
Do not call external APIs.
Do not modify product repos.

Goal:
Create a 90-second storyboard showing that AdMate helps media planners reduce repetitive work and focus on strategy and judgment.

Must cover:
- repetitive policy/check/capture/monitoring pain
- Compass for policy evidence
- Sentinel for validation and monitoring
- Lens for capture/proof
- Foresight for benchmark and forecast
- Agent Core as the shared layer that connects and learns from approved actions
- clear message that AdMate does not replace planners

Tone:
practical, supportive, concise, workflow-first.

Output:
1. 6-8 scene timing table
2. Scene block for each scene
3. On-screen text and captions
4. Presenter cue if useful
5. Disclosure labels where needed
6. Frame QA checklist result
7. Mock visual safety checklist result

Hard constraints:
- no real campaign, advertiser, account, employee, dashboard, token, credential, private URL, or raw data
- no real person imitation
- no internal screenshot
- no generated Lens capture proof
- no animated Sentinel incident action
- no product maturity overclaim
- label mock/concept/aggregate visuals clearly
```

---

## 17. Frame Spec Prompt

Use this when a storyboard scene needs a more precise still-frame specification.

```text
Create a text-only frame spec for the selected AdMate Creative Studio storyboard scene.

Do not generate images or video.
Do not call external APIs.
Do not copy external templates or assets.

Input scene:
- scene_id:
- video_type:
- duration:
- core_message:
- script:
- product_scope:

Output:
- frame purpose
- static layout description
- primary text
- secondary text
- visual object list
- product accent usage
- caption placement
- disclosure placement
- presenter placement if any
- safe mock data labels
- text overflow risks
- product boundary risks
- accessibility notes
- approval checklist

Frame rules:
- layout before animation
- message readable when paused
- no real campaign/client/account data
- no internal dashboard screenshot
- no real person imitation
- no Lens capture output transformation
- no Sentinel critical operation decoration
```

---

## 18. Review Output Template

Design Director review should use this format.

```text
Gate:
Video type:
Storyboard source:
Decision:
Scene count:
Total duration:
P0 blockers:
P1 improvements:
Safety notes:
Product boundary notes:
Disclosure coverage:
Caption/text fit:
Presenter cue review:
Mock visual policy:
No-touch areas confirmed:
Next Gate:
```

Decision values:

- `Pass as text storyboard`
- `Pass with P1 rewrite`
- `Blocked by safety issue`
- `Blocked by product boundary issue`
- `Ready for visual concept Gate`

Next Gate candidates:

```text
Gate CreativeStudio-Executive-Storyboard-QA-1
Gate CreativeStudio-Planner-Storyboard-QA-1
Gate CreativeStudio-Frame-Spec-1
Gate CreativeStudio-Visual-Concept-1
Gate Design-Hyperframes-License-Risk-1
```

---

## 19. What Not To Carry Over From Hyperframes

Do not carry over:

- code
- examples as copied templates
- catalog blocks
- skill files
- package setup
- CLI commands
- render pipeline
- media preprocessing
- external assets
- brand styling

Carry over only:

- structured scene thinking
- timing discipline
- frame-first QA
- agent-readable prompt/rules structure
- clear separation between concept, implementation, render, and QA

Default rule:

```text
Translate ideas into AdMate prompt/rules.
Do not adopt external implementation.
```
