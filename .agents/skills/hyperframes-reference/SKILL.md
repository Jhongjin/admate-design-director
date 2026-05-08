# Hyperframes Reference

Use this skill when reviewing Hyperframes as a reference for AdMate Creative Studio, Design Director video/storyboard planning, frame QA, scene timing, or agent-friendly motion workflow documentation.

This is a reference workflow skill only. It does not install, vendor, copy, run, build, preview, or render Hyperframes.

## Source

- GitHub: https://github.com/heygen-com/hyperframes
- Local inventory: `docs/references/hyperframes-reference-inventory-v1.md`

## Core Rule

Treat Hyperframes as an external reference, not an AdMate dependency.

Do not:

- copy Hyperframes code, docs, skills, templates, catalog blocks, assets, or media into AdMate repos
- install Hyperframes packages or skills
- run npm, bun, preview, render, FFmpeg, Puppeteer, or media commands
- call external APIs
- generate images, videos, voiceover, transcription, or background removal outputs
- add product repo dependencies
- use real campaign, advertiser, account, dashboard, token, or credential data

## When To Use

Use for:

- Creative Studio storyboard planning
- executive intro video outline
- media planner intro video outline
- scene/timing table design
- frame-level QA checklist
- mock-only motion concept notes
- translating external video workflow ideas into AdMate prompt/rules

Do not use for:

- Sentinel critical operations UI
- Lens capture output or preview fidelity
- Compass RAG/source UI
- Foresight raw data visualization with real campaign data
- Homepage production motion implementation without a separate Gate

## Workflow

1. Read `docs/references/hyperframes-reference-inventory-v1.md`.
2. Confirm the task is reference-only and does not require external code, install, render, or asset generation.
3. Translate useful ideas into AdMate-native structure:
   - discovery
   - storyboard
   - frame inventory
   - scene timing
   - message hierarchy
   - Korean text fit
   - mock-only safety note
   - approval/review checklist
4. Keep AdMate Operational Intelligence Console as the design baseline.
5. Limit motion/video exploration to Creative Studio or approved Homepage concept Gates.
6. Explicitly list no-touch areas:
   - product repo code
   - API, DB, auth, env
   - RAG logic
   - Lens capture output
   - Sentinel incident/approval logic
   - secrets and real account/campaign data

## Output Template

```text
Gate:
Reference:
AdMate target:
Use case:
Useful Hyperframes pattern:
AdMate translation:
Forbidden carryover:
Mock/data safety:
License/vendor risk:
No-touch areas:
Next Gate:
```

## License And Copyright Guardrail

Hyperframes is reported in its public repo as Apache-2.0 licensed, but this skill does not authorize copying or vendoring its code or assets.

Before any direct adoption:

- verify license and NOTICE/attribution requirements
- review third-party dependencies and bundled assets
- review runtime requirements such as Node.js, FFmpeg, browser rendering, and Git LFS
- get explicit approval for dependency or code adoption

Default decision: translate ideas into AdMate prompt/rules, not external code.
