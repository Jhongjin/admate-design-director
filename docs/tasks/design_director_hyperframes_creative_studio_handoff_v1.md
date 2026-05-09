# Design Director Hyperframes Creative Studio Handoff v1

작성일: 2026-05-09
Gate: Design-Director-Hyperframes-Handoff-1
대상: AdMate Creative Studio Agent
범위: Design Director repo에 작성된 Hyperframes storyboard prompt pack을 Creative Studio Agent가 사용할 수 있도록 전달 경로, 요약, 반영 방식, 실행 프롬프트를 정리한다. Creative Studio repo는 수정하지 않는다.

---

## 1. 확인 결과

Design Director repo:

```text
D:\Projects\AdMate\admate-design-director
```

Prompt pack 실제 경로:

```text
D:\Projects\AdMate\admate-design-director\docs\prompts\creative-studio-hyperframes-storyboard-prompt-pack-v1.md
```

Git 확인:

```text
branch: main
remote: https://github.com/Jhongjin/admate-design-director.git
commit: 4e994573a3d1a16dbe1a0bd330c4874b11b60869
message: docs: add Creative Studio Hyperframes prompt pack
origin/main: 4e994573a3d1a16dbe1a0bd330c4874b11b60869
```

결론:

```text
creative-studio-hyperframes-storyboard-prompt-pack-v1.md는 Design Director repo의 main에 커밋/푸시되어 있다.
```

---

## 2. Creative Studio Agent가 바로 찾을 수 없는 이유

AdMate repo들은 제품별로 분리되어 있다.

Design Director repo:

```text
D:\Projects\AdMate\admate-design-director
```

Creative Studio repo:

```text
D:\Projects\AdMate\admate-creative-studio
```

Creative Studio Agent가 `admate-creative-studio` repo 안에서 작업하면, 기본적으로 그 repo의 파일만 로컬 작업 맥락으로 본다. Design Director repo의 `docs/prompts/...` 파일은 별도 repo에 있으므로 자동으로 검색되거나 포함되지 않는다.

따라서 Creative Studio Agent에게는 아래 중 하나가 필요하다.

1. Design Director 문서의 절대 경로를 명시해서 읽게 한다.
2. 해당 prompt pack을 Creative Studio repo 안에 local prompt pack으로 별도 작성한다.
3. 중앙 문서 repo 또는 Design Director repo를 reference로 두고 Creative Studio repo에는 짧은 handoff note만 둔다.

---

## 3. Prompt Pack 내용 요약

문서:

```text
docs/prompts/creative-studio-hyperframes-storyboard-prompt-pack-v1.md
```

핵심 목적:

```text
Hyperframes의 HTML-first scene/timing/frame QA 아이디어를
AdMate Creative Studio용 storyboard/script/frame-spec prompt pack으로 번역한다.
```

포함 내용:

- Hyperframes 아이디어의 AdMate식 번역
  - scene as structured source
  - layout before animation
  - timing before render
  - frame QA before production
- executive 5min 영상 구조
- media planner 90s 영상 구조
- scene block schema
- timing checklist
- layout checklist
- caption checklist
- disclosure checklist
- presenter cue checklist
- frame QA checklist
- mock visual safety checklist
- Creative Studio Agent 전달용 prompt
  - executive 5min storyboard prompt
  - planner 90s storyboard prompt
  - frame spec prompt
- Hyperframes에서 carry-over 금지할 것
  - code
  - template
  - catalog block
  - skill files
  - package setup
  - CLI/render/media workflow

---

## 4. Creative Studio repo 반영 방식 권장

권장안:

```text
Creative Studio repo에는 동일 파일을 그대로 복사하기보다,
Creative Studio repo 맥락에 맞는 local prompt pack으로 재작성한다.
```

이유:

- Design Director 문서는 전체 제품군 governance와 금지 영역을 넓게 담는다.
- Creative Studio repo에서는 실제 작업자가 찾기 쉬운 경로, 현재 repo의 route/feature 구조, asset 저장 정책, review workflow를 반영해야 한다.
- 그대로 복사하면 나중에 Design Director 기준과 Creative Studio local 기준이 동시에 drift될 수 있다.
- Creative Studio repo에는 "이 repo에서 실제로 무엇을 해도 되는가"가 더 구체적으로 필요하다.

Creative Studio repo 추천 경로:

```text
docs/prompts/creative-studio-hyperframes-storyboard-prompt-pack-v1.md
```

단, 반영 시에는 아래 원칙을 따른다.

- Design Director 원문을 무단 vendor처럼 복사하지 않고 Creative Studio용 local prompt pack으로 재작성한다.
- Hyperframes 외부 code/template/catalog/skill/package/CLI/render workflow는 복사하지 않는다.
- 영상/image/render 산출물은 만들지 않는다.
- asset/media 파일은 추가하지 않는다.
- 제품 구현 코드는 별도 승인 Gate 전 수정하지 않는다.
- Design Director source 문서와 commit hash를 reference로 남긴다.

대안:

```text
Creative Studio repo에 짧은 handoff note만 추가하고,
실제 prompt pack은 Design Director repo 절대 경로를 읽게 한다.
```

이 방식은 중복을 줄이지만, Creative Studio Agent가 독립 repo 작업 중 경로를 놓칠 수 있다.

---

## 5. Creative Studio Agent 전달용 실행 프롬프트

아래 프롬프트를 Creative Studio Agent에게 그대로 전달한다.

```text
너는 AdMate Creative Studio Agent다.

목표:
Design Director repo에 작성된 Hyperframes storyboard prompt pack을 Creative Studio repo에서 사용할 수 있는 local prompt pack으로 반영할지 검토하고, 승인된 경우 Creative Studio용 문서로 재작성한다.

읽을 Design Director reference:
- D:\Projects\AdMate\admate-design-director\docs\prompts\creative-studio-hyperframes-storyboard-prompt-pack-v1.md
- D:\Projects\AdMate\admate-design-director\docs\references\hyperframes-reference-inventory-v1.md
- D:\Projects\AdMate\admate-design-director\.agents\skills\hyperframes-reference\SKILL.md

Design Director source commit:
- 4e994573a3d1a16dbe1a0bd330c4874b11b60869
- docs: add Creative Studio Hyperframes prompt pack

Creative Studio repo 추천 산출물:
- docs/prompts/creative-studio-hyperframes-storyboard-prompt-pack-v1.md

작업 원칙:
- 먼저 Creative Studio repo의 AGENTS.md, README.md, docs 구조를 읽고 현재 문서 위치와 규칙을 확인한다.
- 바로 파일을 수정하지 말고, 반영 후보 경로와 작업 계획을 먼저 보고한다.
- Design Director prompt pack을 그대로 복사하기보다 Creative Studio repo 맥락에 맞는 local prompt pack으로 재작성하는 것을 우선 검토한다.
- executive 5min / planner 90s storyboard, scene block, timing, layout, caption, disclosure, presenter cue, frame QA, mock visual safety checklist를 유지한다.
- Design Director source path와 commit hash를 reference로 명시한다.

금지:
- Hyperframes code, package, plugin, skill, template, catalog block, asset, CLI/render workflow를 복사하지 않는다.
- npm install/build/render 실행 금지.
- 영상 생성 금지.
- image generation 금지.
- 외부 API 호출 금지.
- product feature code 수정 금지.
- asset/media 파일 추가 금지.
- 실제 광고주명, 캠페인명, 계정정보, 내부 dashboard screenshot, token, credential 사용 금지.
- Lens capture output 또는 Sentinel critical operation workflow에 영상/motion workflow를 적용하지 않는다.

권장 보고 형식:
Gate:
Repo:
Read files:
Recommended path:
Copy vs local rewrite decision:
Planned content:
No-touch areas:
Risks:
Validation plan:
Next action:
```

---

## 6. Creative Studio 반영 체크리스트

Creative Studio repo에 반영하기 전 확인:

- [ ] Creative Studio repo의 `AGENTS.md`와 `README.md`를 읽었다.
- [ ] Creative Studio repo의 docs 구조를 확인했다.
- [ ] Design Director source path와 commit hash를 기록했다.
- [ ] 그대로 복사할지 local rewrite할지 결정했다.
- [ ] 외부 code/vendor가 포함되지 않는다.
- [ ] Hyperframes package/CLI/render workflow가 포함되지 않는다.
- [ ] image/video/asset 생성 없이 문서만 작성한다.
- [ ] executive 5min과 planner 90s 구조를 유지한다.
- [ ] mock visual safety checklist를 포함한다.
- [ ] 실제 내부 정보와 raw campaign data 금지를 명시한다.
- [ ] Lens/Sentinel 금지 영역을 명시한다.
- [ ] commit/push는 별도 승인 후 진행한다.

---

## 7. Design Director 권장 결정

권장:

```text
Creative Studio repo에는 local prompt pack으로 재작성한다.
```

권장 이유:

- Creative Studio Agent가 같은 repo 안에서 바로 찾을 수 있다.
- Design Director의 넓은 governance를 Creative Studio 작업 흐름에 맞게 줄일 수 있다.
- future visual concept Gate에서 산출물, asset policy, review 상태를 Creative Studio repo 문맥으로 관리하기 쉽다.

단, 첫 반영 Gate에서는 문서만 작성한다.

다음 Gate 추천:

```text
Gate CreativeStudio-Hyperframes-LocalPrompt-1
```

목표:

- Creative Studio repo에서 local prompt pack 문서 작성.
- 영상/image/render/asset 생성 없이 storyboard prompt와 QA checklist만 반영.
- Design Director source path와 commit hash를 reference로 기록.
