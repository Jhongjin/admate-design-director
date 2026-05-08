# Hyperframes Reference Inventory v1

작성일: 2026-05-09
Gate: Design-Director-Hyperframes-1
대상 repo: https://github.com/heygen-com/hyperframes
범위: read-only reference inventory. Hyperframes 코드, asset, dependency, plugin, skill을 AdMate repo에 vendor/copy하지 않는다.

---

## 1. 확인 범위

확인한 공개 GitHub 자료:

- GitHub repo: https://github.com/heygen-com/hyperframes
- README: https://github.com/heygen-com/hyperframes/blob/main/README.md
- License: https://github.com/heygen-com/hyperframes/blob/main/LICENSE
- Design note: https://github.com/heygen-com/hyperframes/blob/main/DESIGN.md
- Package manifest: https://github.com/heygen-com/hyperframes/blob/main/package.json
- Skills directory: https://github.com/heygen-com/hyperframes/tree/main/skills
- Hyperframes skill: https://github.com/heygen-com/hyperframes/blob/main/skills/hyperframes/SKILL.md

이번 Gate에서는 GitHub 문서 확인만 수행했다.

하지 않은 것:

- repo clone
- npm/bun install
- build/render/preview 실행
- 외부 API 호출
- 이미지/영상 생성
- 코드/asset/vendor copy

---

## 2. Repo 목적 요약

Hyperframes는 HTML 기반 video composition을 만들고 preview/render하는 open-source video rendering framework다.

핵심 방향:

```text
Write HTML.
Render video.
Built for agents.
```

README 기준 주요 특징:

- HTML file을 video composition의 source로 사용한다.
- `data-*` attributes로 timing, track, composition metadata를 표현한다.
- browser preview와 MP4 render workflow를 제공한다.
- AI coding agent가 composition, timeline, styling, adapter animation을 작성하기 쉽게 skill/plugin 구조를 제공한다.
- GSAP, Lottie, CSS animation, Three.js, WAAPI 같은 runtime adapter를 frame-accurate workflow에 연결하는 패턴을 제공한다.

AdMate 관점의 해석:

```text
Hyperframes는 AdMate 제품 UI framework가 아니다.
Creative Studio의 영상 storyboard, scene timing, frame QA, motion prompt 구조를 설계할 때 참고할 수 있는 영상 제작 workflow reference다.
```

---

## 3. 주요 기능

공개 README와 repo 구조 기준 주요 기능:

| Area | 내용 | AdMate reference value |
|---|---|---|
| HTML composition | HTML과 `data-*` attributes로 scene/timing/media를 정의 | storyboard frame schema와 scene metadata 설계 참고 |
| Browser preview | composition을 browser에서 preview | Creative Studio preview QA flow 참고 |
| Render pipeline | local/Docker render to MP4 방향 | 직접 도입 전 보안/성능/운영 검토 필요 |
| Frame adapter pattern | GSAP, Lottie, CSS, Three.js, WAAPI 등 animation runtime 연결 | motion runtime 선택 기준과 adapter boundary 참고 |
| Skills | agent에게 video composition 작성/CLI/media workflow를 가르치는 skill package | AdMate 전용 prompt/rules 구조 참고 |
| Catalog/blocks | social overlays, shader transitions, data visualization 등 reusable blocks | 구조 참고만 가능, code/asset copy 금지 |
| Studio/player packages | browser editor UI와 embeddable player package | Creative Studio long-term ideation reference |

---

## 4. 설치 / 사용 방식

README 기준 사용 방식은 크게 두 갈래다.

### 4.1 Agent-first

Hyperframes는 AI coding agent가 composition authoring과 dev-loop를 이해하도록 skills/plugin을 제공한다.

README에서 언급된 설치/연결 방식:

- skills install: `npx skills add heygen-com/hyperframes`
- Codex plugin sparse install
- Claude/Cursor plugin workflow

AdMate 적용 판단:

- 이 Gate에서는 설치하지 않는다.
- Design Director repo에는 외부 skill을 그대로 설치하지 않는다.
- 필요한 경우 Hyperframes repo의 workflow를 AdMate 전용 prompt/rules로 번역한다.

### 4.2 Manual project

README 기준 manual flow:

- init
- browser preview
- MP4 render

AdMate 적용 판단:

- 이번 Gate에서는 실행하지 않는다.
- 직접 도입 전 Node.js, FFmpeg, Git LFS, browser rendering, media handling, security review가 필요하다.

---

## 5. 라이선스

GitHub repo는 Apache License 2.0으로 표시되어 있다.

AdMate 적용 판단:

- Apache-2.0은 일반적으로 상업적 사용과 수정/배포를 허용하는 permissive open-source license다.
- 하지만 이 Gate에서는 외부 코드를 복사하거나 dependency로 도입하지 않는다.
- 직접 도입 전 반드시 license notice, attribution, NOTICE file 여부, bundled third-party assets/dependencies, media/model license를 별도 확인해야 한다.

주의:

- README, docs, skills, catalog examples를 그대로 복사하지 않는다.
- Hyperframes 코드나 template을 AdMate repo에 vendor하지 않는다.
- Apache-2.0이라도 AdMate 사내/제품 정책에 맞는 legal/security review 전에는 제품 repo dependency로 추가하지 않는다.

---

## 6. 디자인 / 영상 / 프레임 제작에서 참고할 수 있는 부분

### 6.1 Scene as structured source

Hyperframes는 HTML과 attributes를 통해 scene, media, timing을 구조화한다.

AdMate translation:

- Creative Studio storyboard frame을 `scene_id`, `duration`, `message`, `visual_role`, `voiceover`, `caption`, `evidence_source`, `risk_note` 같은 structured spec으로 먼저 설계한다.
- 영상 생성 전 frame inventory와 timing table을 문서화한다.

### 6.2 Layout before animation

Hyperframes skill은 animation 전에 가장 중요한 frame의 static layout을 먼저 잡는 접근을 강조한다.

AdMate translation:

- executive/media planner video storyboard에서 hero frame을 먼저 정의한다.
- motion보다 message clarity, Korean text fit, product boundary, mock-only labeling을 먼저 QA한다.
- Sentinel/Lens 같은 운영 영역으로 motion workflow가 번지지 않게 한다.

### 6.3 Agent-friendly workflow

Hyperframes는 agent가 이해할 수 있는 skill, guide, adapter 분리를 제공한다.

AdMate translation:

- Design Director가 Creative Studio Agent에게 넘길 prompt/rules를 `Discovery -> Storyboard -> Frame QA -> Motion plan -> Safety review` 형태로 분리한다.
- skill은 external implementation이 아니라 internal review guide로 둔다.

### 6.4 Frame adapter boundary

Hyperframes는 여러 animation runtime을 adapter로 분리한다.

AdMate translation:

- AdMate는 runtime 선택보다 "어떤 화면에 motion이 허용되는가"를 먼저 정의한다.
- Creative Studio와 Homepage visual exploration만 후보로 둔다.
- Sentinel critical operations와 Lens capture output에는 motion/video workflow를 적용하지 않는다.

### 6.5 Data visualization and explainer video

Hyperframes catalog에는 animated data visualization과 social/video overlay 계열이 있다.

AdMate translation:

- Foresight benchmark 설명 영상, executive brief, media planner intro storyboard의 구조 참고 가능.
- 실제 raw campaign-level data는 사용하지 않는다.
- mock/aggregate data만 사용한다.

---

## 7. AdMate 활용 후보

| Candidate | Value | Gate |
|---|---|---|
| Creative Studio storyboard prompt pack | frame/timing/message structure를 agent-friendly하게 정리 | `Gate CreativeStudio-Hyperframes-Storyboard-1` |
| Executive intro video outline | AdMate 제품군과 Agent Core를 짧은 sequence로 설명 | `Gate CreativeStudio-Executive-Video-1` |
| Media planner intro storyboard | 반복 업무, Compass/Sentinel/Lens/Foresight lifecycle를 설명 | `Gate CreativeStudio-Planner-Video-1` |
| Homepage hero motion concept | static section visual 또는 lightweight concept only | `Gate Homepage-Visual-Exploration-1` |
| Foresight benchmark explainer | mock/aggregate data 기반 report explanation video outline | `Gate Foresight-Explainer-Storyboard-1` |
| Creative Studio QA checklist | frame text overflow, timing, evidence, mock safety checklist | `Gate CreativeStudio-Frame-QA-1` |

---

## 8. 적용하면 안 되는 부분

AdMate에 적용 금지:

- Hyperframes code, package, plugin, skill, template, asset을 그대로 vendor/copy
- product repo dependency 추가
- npm/bun install 또는 render pipeline 실행
- 실제 campaign data, advertiser name, account information이 포함된 video generation
- Lens capture output을 video/frame workflow로 변형
- Sentinel critical operations에 animation-heavy workflow 적용
- internal dashboard screenshot을 영상 asset으로 사용
- HeyGen/Hyperframes brand, demo asset, catalog block을 AdMate asset처럼 사용
- TTS, transcription, background removal, external media workflow를 승인 없이 사용

특히 Lens:

```text
Lens capture output fidelity는 Hyperframes reference와 무관하다.
Capture result pixel, platform template, preview fidelity는 절대 변경하지 않는다.
```

특히 Sentinel:

```text
Incident, approval, fail-closed UI에는 motion/video-first workflow를 적용하지 않는다.
Critical operation clarity와 auditability가 우선이다.
```

---

## 9. 금지 / 주의점

### 9.1 External code and copyright

- 외부 repo 코드를 AdMate repo에 복사하지 않는다.
- README, docs, skill 내용을 장문 복사하지 않는다.
- Hyperframes의 구조적 아이디어만 AdMate 전용 prompt/rules로 번역한다.
- 직접 도입 전 license notice와 attribution 정책을 확인한다.

### 9.2 Dependency and runtime

직접 도입 전 확인:

- Node.js version requirement
- FFmpeg requirement
- browser rendering security
- Puppeteer/headless Chrome 운영 리스크
- Git LFS large media files
- render output storage 정책
- CI/CD 비용과 runtime 시간
- Windows local workflow compatibility
- media codec/legal constraints

### 9.3 Data safety

금지:

- secret/env/token 출력
- real advertiser/campaign/account data 사용
- private dashboard screenshot 사용
- raw campaign-level data 기반 영상 생성
- signed URL/private media 사용

허용:

- mock visual
- aggregate metric
- anonymized scenario
- public-safe brand story
- Design Director 문서용 storyboard spec

---

## 10. 직접 도입 전 확인할 것

직접 도입 전 checklist:

- [ ] Apache-2.0 license와 NOTICE/attribution 요구 확인
- [ ] third-party dependency license 확인
- [ ] bundled assets, fonts, media, model weights license 확인
- [ ] Node/FFmpeg/Puppeteer runtime 보안 검토
- [ ] render output 저장 위치와 retention 정책 확인
- [ ] Git LFS 대용량 파일 유입 방지 정책 확인
- [ ] product repo dependency 추가 필요성 검토
- [ ] Creative Studio sandbox와 product runtime repo 분리
- [ ] mock-only data policy 확인
- [ ] 이미지/영상 생성물 review/approval flow 정의
- [ ] Sentinel/Lens 금지 영역 재확인

---

## 11. Design Director 결론

Hyperframes는 AdMate Design Director / Creative Studio reference로 쓸 가치가 있다.

가치 있는 이유:

- HTML-first scene authoring과 structured timing model이 agent-friendly하다.
- skill-based workflow가 Design Director의 prompt/rules 관리 방식과 잘 맞는다.
- storyboard, frame QA, motion planning을 문서화하는 데 참고하기 좋다.
- Creative Studio의 executive/media planner intro video 설계에 적용 가능한 패턴이 많다.

제한:

- 지금은 reference inventory 수준으로만 둔다.
- 제품 repo dependency로 추가하지 않는다.
- Hyperframes code/asset/template을 복사하지 않는다.
- Creative Studio sandbox 밖으로 영상 workflow를 확산하지 않는다.

권장 다음 Gate:

```text
Gate CreativeStudio-Hyperframes-Storyboard-1
```

목표:

- Hyperframes의 scene/timing/frame QA 아이디어를 AdMate Creative Studio 전용 storyboard prompt pack으로 번역한다.
- 영상 생성은 하지 않고 frame spec, script block, safety checklist만 작성한다.

보조 Gate:

```text
Gate Design-Hyperframes-License-Risk-1
```

목표:

- 직접 도입 가능성을 검토하기 전 license, dependency, asset, runtime risk를 별도 점검한다.
