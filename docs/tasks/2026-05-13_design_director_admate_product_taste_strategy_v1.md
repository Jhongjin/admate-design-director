# Design Director AdMate Product Taste Strategy v1

작성일: 2026-05-13
문서 상태: draft
Gate: Design-Director-AdMate-Product-Taste-Strategy-1

## 1. Scope

이 문서는 AdMate 제품군의 디자인 방향을 정하는 Design Director 전략 산출물이다.

허용 범위:

- 기존 AdMate design system, product UI direction, reference log 검토
- taste-skill, Neuform, Awesome Designed류 외부 reference의 역할 정의
- Homepage, Compass, Sentinel, Lens, Foresight별 디자인 방향과 main page/cockpit 필요성 정리
- 이후 제품별 Agent에게 전달할 implementation gate 순서 제안

금지 범위:

- 제품 repo 코드 수정
- 외부 template, code, asset, vendor import
- Taste Skill 설치 또는 제품 repo 적용
- API, DB, auth, RAG, capture engine, monitoring logic 변경
- production smoke, SQL, n8n 실행
- secret, token, cookie, session, credential, environment value 출력

## 2. Inputs Reviewed

Local baseline:

- `docs/design-system/admate-design-system-v1.md`
- `docs/references/neuform-reference-log-v1.md`
- `docs/references/taste-skill-ai-design-workflow-notes-v1.md`
- `docs/reviews/design-gate-priority-map-v1.md`
- product UI direction docs for Compass, Sentinel/Openclaw, Lens, Foresight, Homepage
- Homepage `DESIGN.md`
- Compass `compass-ui-3-page-layout-source-ux-plan`
- sub-agent read-only design audits for local references, product positioning, and frontend risk

External references:

- taste-skill GitHub: `https://github.com/Leonxlnx/taste-skill`
- Neuform Community Featured: `https://neuform.ai/community/featured`

Local note:

- No local file named `Awesome designed.md` was found during read-only inventory.
- The existing AdMate reference stack already includes Linear, Notion, Mintlify, IBM Carbon, Sentry, Airtable, Vercel, and minimal Apple-like spacing as product-specific references.

## 3. Core Decision

AdMate should not choose one external theme and apply it everywhere.

The right model is:

```text
AdMate Operational Intelligence Console
+ product-specific taste board
+ taste-skill as critique/process layer
+ Neuform as reference gallery
+ existing product references as local baseline
```

Reasoning:

- Homepage, Compass, Sentinel, Lens, and Foresight serve different users and different moments of trust.
- A single purple/dark "AI SaaS" skin would make the platform feel generic and would hide the professional signal the user wants.
- A full Neuform or taste-skill transplant would create visual energy, but could break evidence, audit, source, capture fidelity, or operational density.
- The existing AdMate design system is already directionally correct: calm, operational, evidence-first, traceable, with muted product accents.

Therefore, the design work should deepen product identity without breaking the shared system.

## 4. Design Thesis

AdMate's first impression should be professional competence.

That means:

- The page shows what the product does before it explains itself.
- The user can immediately see current state, next action, evidence, risk, or data readiness.
- Product accents signal domain and mode, but do not dominate the screen.
- Empty states are not blank. They explain readiness, next input, or missing evidence.
- Main pages are not marketing landing pages. They are product cockpits.
- Visual richness should come from hierarchy, data shape, real workflow context, and exact copy, not decorative blobs, heavy gradients, or generic AI hero sections.

## 5. Product Taste Map

### Homepage / Command Center

Role:

- Executive view of the AdMate product ecosystem.
- Shows cross-product progress, status, handoff maturity, and weekly operating confidence.

Baseline reference:

- Vercel + Linear + Notion.

Taste direction:

- Confident, connected, editorially restrained.
- Should feel like an operating system for advertising intelligence, not a SaaS brochure.
- Should not feel like a product feature manual. The first impression should be a decision room: current operating picture, product confidence, blockers, and what needs attention.

Main page/cockpit:

- Platform identity
- Product cards with progress, status, owner, next milestone
- Current week summary
- Blockers and decision queue
- One clear path into each product

Avoid:

- Oversized marketing hero
- Abstract gradient hero
- Decorative AI diagrams with no operational meaning
- Feature-guide composition that lists what each platform does without showing the state of the business
- Internal admin leakage on public surfaces

### Compass

Role:

- Policy Intelligence and evidence-backed answer workspace.

Baseline reference:

- Mintlify + Notion + Linear.

Taste direction:

- Calm documentation intelligence.
- Source and citation panels are the professional signal.

Main page/cockpit:

- Ask policy question
- Current evidence/source health
- Recent platform policy updates
- Recent questions or saved answer states
- Generation-limited/noData states with explicit cause

Useful Neuform pattern type:

- AI answer cards
- documentation source cards
- source drawer / evidence panel

Avoid:

- Chatbot-only first impression
- Decorative source cards that reduce source credibility
- Design changes that touch RAG, route, DB, crawler, import, or noData logic

### Sentinel

Role:

- Campaign preflight, live monitoring, alert, operator action, approval, and audit.

Baseline reference:

- IBM Carbon + Linear + Sentry.

Taste direction:

- Critical operations clarity.
- Dense but calm, with status and audit above decoration.

Main page/cockpit:

- Live/prelaunch status
- Alert queue
- Latest ingest/read-model state
- Operator actions
- Recent delivery and audit results
- Fail-closed and disabled states clearly visible

Useful Neuform pattern type:

- Monitoring console
- incident timeline
- approval queue
- operational audit panel

Avoid:

- Cinematic dark UI as default
- Motion-heavy incident screens
- Beautiful but ambiguous status badges
- Hiding disabled/fail-closed state inside neutral copy

### Lens

Role:

- Capture and proof generation for ad placements and reporting.

Baseline reference:

- Linear + Sentry + minimal Apple-like spacing.

Taste direction:

- Visual production console.
- Operator UI can be polished, but capture output fidelity must remain untouched.

Main page/cockpit:

- Capture queue
- Running/completed/failed jobs
- Golden sample and QA status
- Preview detail
- Cancel/retry controls
- Evidence/export readiness

Useful Neuform pattern type:

- Quality dashboard
- image review grid
- queue/list detail split
- inspection panel

Avoid:

- Styling captured media output
- Cropping, filtering, or beautifying placement output
- Making screenshots look more polished than the real capture artifact

### Foresight

Role:

- Performance forecast, benchmark interpretation, media planning support.

Baseline reference:

- Airtable + IBM Carbon + Linear.

Taste direction:

- Strategic analyst cockpit.
- Looks like a forecast workbench, not a magical prediction app.

Main page/cockpit:

- Forecast simulator
- Benchmark readiness
- Data coverage and confidence
- Industry/goal/period filters
- Scenario history
- Account/session state

Useful Neuform pattern type:

- Analytics dashboard
- table upload flow
- empty-state dashboard
- benchmark comparison card

Avoid:

- Futuristic AI prediction visuals
- Overclaiming confidence
- Empty chart cards with no next action
- Hiding that data is insufficient or still being standardized

## 6. Taste Skill Position

Taste Skill should not become the AdMate design system.

Recommended use:

- Design Director reference only
- Translate into AdMate prompt/rules
- Use as a critique checklist against generic AI UI
- Apply per product through explicit implementation gates

Recommended dials:

| Surface | Design variance | Motion intensity | Visual density |
|---|---:|---:|---:|
| Homepage | medium | low-medium | medium |
| Compass | low-medium | low | medium-high |
| Sentinel | low | very low | high |
| Lens operator UI | medium | low | medium-high |
| Lens capture output | none | none | unchanged |
| Foresight | medium | low | medium |

Taste Skill is most useful for:

- Better typography rhythm
- Less generic card hierarchy
- Stronger screen composition
- Better mobile polish
- More deliberate empty states

Taste Skill is not useful for:

- Replacing product logic
- Changing evidence/source contracts
- Changing capture outputs
- Generating unapproved assets for production

## 7. Neuform Position

Neuform should be used as a gallery of product mood and layout patterns, not a template source.

Allowed extraction:

- Layout structure
- Density
- Component arrangement
- Empty state treatment
- Review/detail panel layout
- Dashboard hierarchy

Disallowed carry-over:

- Brand color
- Gradient/glass style
- Decorative illustration
- Marketing-first hero composition
- Any code/template/asset/vendor copied into product repos

Recommended translation:

```text
Neuform reference
→ visual trait summary
→ product-specific AdMate rule
→ implementation prompt
→ screenshot QA checklist
```

## 8. Implementation Queue Recommendation

### Phase 1. Product Cockpit Contracts

Create docs-only cockpit contracts for:

- Foresight main page and empty/data-ready states
- Lens capture queue and QA review surface
- Compass answer/source main workspace
- Sentinel monitoring and prelaunch status cockpit
- Homepage Command Center v2

Reason:

- This locks the experience before code changes.
- It also keeps product agents from applying generic style changes independently.

### Phase 2. Implementation Order

Recommended order:

1. Foresight cockpit polish
   - Lowest current product maturity.
   - Auth/handoff is now usable.
   - Empty states need professional analyst framing.

2. Lens operator queue and review polish
   - Capture reliability and golden sample work are active.
   - Must preserve output fidelity while improving operator confidence.

3. Compass evidence workspace polish
   - Functionally mature.
   - Biggest trust gain comes from evidence/source hierarchy.

4. Sentinel monitoring cockpit polish
   - Operationally sensitive.
   - Proceed after producer/read-model gates are stable.

5. Homepage Command Center v2
   - Use after product cards have accurate progress/status.
   - It should reflect product reality, not aspirational marketing.

### Phase 3. Visual Reference Boards

For each product, create a small reference board document:

- target mood
- 3 approved external reference traits
- 3 rejected traits
- product-specific prompt pack
- component/state checklist
- mobile screenshot checklist

## 9. Product Agent Handoff Rules

Each product implementation agent should receive:

- product role
- target user
- target first viewport
- allowed files/surfaces
- explicit no-touch areas
- screenshot viewports
- copy rules
- regression checks

Required no-touch confirmations:

- No DB/schema changes
- No auth/session changes
- No API contract changes
- No RAG/search/crawler/import changes
- No capture output transformation
- No production calls unless separately approved

## 10. Human Decision Points

Human approval is needed for:

- Final visual reference board selection per product
- Any generated visual asset used in production
- Any product repo implementation gate
- Any production smoke

Commander can proceed without human approval for:

- docs-only strategy
- reference inventory
- product cockpit contract draft
- static UI risk audit
- implementation prompt pack
- screenshot checklist

## 11. Current Recommendation

Use the existing AdMate Operational Intelligence Console as the spine.

Do not choose between Awesome Designed, taste-skill, and Neuform as if they are mutually exclusive themes.

Instead:

```text
Existing AdMate system = foundation
Neuform = product-specific visual references
taste-skill = anti-generic critique and refinement workflow
Awesome/Linear/Carbon/Mintlify/etc. = already-selected baseline references
```

This gives AdMate the professional identity the user is asking for: not generic AI purple, not bland admin UI, and not a copied template, but a product family whose screens look like they understand the work they are built to support.

## 12. Verification Plan

For this strategy gate:

- `git diff --check`
- secret-like value scan
- staged file check

For future product implementation gates:

- desktop screenshot
- mobile screenshot
- text overflow check
- source/evidence/status/action hierarchy review
- no-touch confirmation for product logic
- existing product verification commands
