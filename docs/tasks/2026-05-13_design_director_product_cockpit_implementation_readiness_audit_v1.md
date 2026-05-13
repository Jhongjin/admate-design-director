# Design Director Product Cockpit Implementation Readiness Audit v1

Date: 2026-05-13

Scope: AdMate Homepage / Command Center, Compass, Sentinel, Lens, and Foresight cockpit implementation readiness.

Mode: read-only implementation readiness audit. No product code was changed in this Gate.

## Commander Decision

Status: proceed with phased cockpit implementation.

The Design Director cockpit strategy is ready to leave pure planning, but each product should move in a different implementation lane. A single shared visual skin would make the suite feel generic. The better direction is shared operating grammar with product-specific information density, motion, and surfaces.

The homepage must stop reading like a feature manual. It should become an executive operating cockpit that answers: what is running, what is blocked, where confidence is changing, and what needs a decision.

## Shared Design Contract

- Shared grammar: compact status chips, sober progress cues, accountable timestamps, visible state changes, and operator-oriented summaries.
- Product-specific expression: each product can use a different density, contrast, layout rhythm, and interaction model when the product job requires it.
- No generic AI treatment: avoid purple/dark-first styling as a default identity. Purple can remain as a controlled accent, not the whole personality.
- No decorative dashboard theater: every visual element needs an operational meaning.
- No raw secrets, tokens, cookies, session payloads, credentials, raw provider responses, or unredacted production payloads in UI or docs.

## Homepage / Command Center

Readiness: ready for implementation planning.

Observed candidate surfaces:

- `D:\Projects\AdMate\admate-homepage\src\app\page.tsx`
- `D:\Projects\AdMate\admate-homepage\src\app\command-center\page.tsx`
- `D:\Projects\AdMate\admate-homepage\src\components\command-center\CommandCenterPage.tsx`
- `D:\Projects\AdMate\admate-homepage\src\components\command-center\ProjectProgressCard.tsx`
- `D:\Projects\AdMate\admate-homepage\src\components\command-center\SummaryCards.tsx`
- `D:\Projects\AdMate\admate-homepage\src\lib\command-center-data.ts`

Recommended next Gate: Homepage-CommandCenter-Cockpit-Implementation-1.

Implementation direction:

- Turn the first viewport into a mission-control overview rather than a product brochure.
- Use an executive status strip, decision queue, product health matrix, and a short "changed since last review" rail.
- Keep public-safe fields limited to broad readiness, redacted milestones, non-sensitive next-step labels, and aggregated status.
- Keep internal owner, blockers, decision history, and stale/live basis behind the authenticated Command Center boundary.

Risks:

- Over-explaining the product suite would preserve the current feature-manual feeling.
- Connecting live product internals too early could blur public/auth boundaries.

No-touch boundary for next implementation:

- No production DB mutation.
- No secret/env readback.
- No new public raw operational payloads.
- No live product API dependency unless a separate integration gate approves it.

## Compass

Readiness: implementation pass complete; needs design QA and selective polish.

Observed candidate surfaces:

- `D:\Projects\AdMate\admate-compass\src\app\page.tsx`
- `D:\Projects\AdMate\admate-compass\src\app\chat-ollama\page.tsx`

Current state:

- Compass already moved toward a policy evidence desk with answer, evidence, and source review structure.
- The next useful design work is not a redesign from scratch, but density, hierarchy, and no-data state polish.

Recommended next Gate: Compass-Cockpit-Design-QA-1.

Implementation direction:

- Keep Compass as a policy evidence workspace, not a conversational toy.
- Improve source panel hierarchy, confidence badges, and generation-limited/noData affordances.
- Verify mobile evidence ordering and source readability after each visual change.

Risks:

- Too much visual drama could reduce trust in policy answers.
- Source cards must remain scannable under long Korean policy text.

No-touch boundary:

- No RAGSearchService logic changes.
- No `/api/chat-ollama` behavior changes.
- No DB/schema/import/reembedding/crawler changes.

## Sentinel

Readiness: ready for focused cockpit surface planning, while live producer workflow remains controlled.

Observed candidate surfaces:

- `D:\Projects\AdMate\admate-sentinel-legacy\src\app\(dashboard)\page.tsx`
- `D:\Projects\AdMate\admate-sentinel-legacy\src\app\(dashboard)\active\ActiveDashboardClientUI.tsx`
- `D:\Projects\AdMate\admate-sentinel-legacy\src\app\(dashboard)\audit\AuditClientUI.tsx`
- `D:\Projects\AdMate\admate-sentinel-legacy\src\components\Sidebar.tsx`
- `D:\Projects\AdMate\admate-agent-core\src\app\api\sentinel\prelaunch\status\route.ts`
- `D:\Projects\AdMate\admate-agent-core\src\app\api\sentinel\prelaunch\ingest\route.ts`

Current state:

- Prelaunch ingest schema/read-model exists in production.
- Controlled manual n8n producer smoke succeeded after credential wiring.
- Model A workflow activation readback showed no Sentinel prelaunch row delta, which matches the approved expectation.

Recommended next Gate: Sentinel-Cockpit-Prelaunch-Status-Surface-1.

Implementation direction:

- Surface Sentinel as an operations console: live monitoring, evidence of last check, alert lifecycle, and producer readiness.
- The cockpit should show "what is watching", "what changed", and "what action is safe now".
- Keep n8n producer state visually separate from campaign monitoring state.

Risks:

- Mixing controlled smoke rows and real campaign checks can confuse operators.
- Producer branch must remain default-off until a separate live producer approval gate.

No-touch boundary:

- No n8n activation changes.
- No production ingest requests.
- No SQL mutation.
- No credential readback.
- No workflow execution unless separately approved.

## Lens

Readiness: ready for operator cockpit planning; capture cancellation and golden fixture surfacing are high-value next steps.

Observed candidate surfaces:

- `D:\Projects\AdMate\admate-lens\src\app\LensHomePageClient.tsx`
- `D:\Projects\AdMate\admate-lens\src\app\components\CaptureForm.tsx`
- `D:\Projects\AdMate\admate-lens\src\app\components\CaptureList.tsx`
- `D:\Projects\AdMate\admate-lens\src\lib\capture\abort-registry.ts`
- `D:\Projects\AdMate\admate-lens\src\lib\capture\batch-serverless.ts`
- `D:\Projects\AdMate\admate-lens\src\lib\capture\channels\gdn-capture.ts`
- `D:\Projects\AdMate\admate-lens\src\lib\capture\channels\youtube-capture.ts`

Current state:

- Golden candidates were approved for controlled Lens fixtures.
- User-observed pain points remain: long-running media capture, repeated media entries, and need for a visible stop/cancel workflow.

Recommended next Gate: Lens-Capture-Operator-Cockpit-1.

Implementation direction:

- Treat Lens as a capture operations bay: queue, current worker, cancellation, retry state, golden sample confidence, and channel-specific diagnostics.
- Expose cancel/stop as an operator control when server/runtime support is already present or can be safely wired.
- Present golden samples as sanitized capture quality references, not media-brand proof.

Risks:

- Live publisher pages and YouTube surfaces carry brand/media context and should not become portable golden assets without sanitization.
- Capture cancellation must not leave storage records or browser sessions in ambiguous states.

No-touch boundary:

- No production capture execution.
- No storage deletion.
- No external media scraping during design docs.
- No new generated media unless separately approved.

## Foresight

Readiness: ready for first cockpit foundation, but data-empty states need product-grade treatment.

Observed candidate surfaces:

- `D:\Projects\AdMate\admate-foresight\app\page.tsx`
- `D:\Projects\AdMate\admate-foresight\app\SimulatorPageClient.tsx`
- `D:\Projects\AdMate\admate-foresight\app\trends\TrendsPageClient.tsx`
- `D:\Projects\AdMate\admate-foresight\app\insights\InsightsPageClient.tsx`
- `D:\Projects\AdMate\admate-foresight\app\competitor\CompetitorPageClient.tsx`
- `D:\Projects\AdMate\admate-foresight\components\Navigation.tsx`
- `D:\Projects\AdMate\admate-foresight\components\StatePanel.tsx`
- `D:\Projects\AdMate\admate-foresight\lib\auth\foresightSession.ts`

Current state:

- Foresight production auth handoff is working.
- Protected pages and logout behavior are verified.
- The core product issue is still data-empty analytics surfaces. The design should make "not enough data yet" feel intentional, not broken.

Recommended next Gate: Foresight-Cockpit-Empty-State-Foundation-1.

Implementation direction:

- Make the top surface a media planning cockpit: scenario inputs, forecast confidence, benchmark readiness, and missing-data checklist.
- Keep empty charts, but pair them with structured requirements and next safe actions.
- Add a simple product main page that orients users before deep charts.

Risks:

- Empty dashboards can look unfinished even when auth and shell are correct.
- Prediction screens must not imply model confidence without benchmark data.

No-touch boundary:

- No Meta API calls.
- No benchmark import/upload.
- No Python retrain.
- No DB/schema/env changes.

## Sequencing

1. Homepage-CommandCenter-Cockpit-Implementation-1
2. Foresight-Cockpit-Empty-State-Foundation-1
3. Lens-Capture-Operator-Cockpit-1
4. Sentinel-Cockpit-Prelaunch-Status-Surface-1
5. Compass-Cockpit-Design-QA-1

Rationale:

Homepage should establish the suite-level operating posture first. Foresight then needs the strongest product perception lift because the authenticated shell is working but the analytics surface is data-empty. Lens has strong operator pain points and should get queue/cancel/golden visibility next. Sentinel should then connect prelaunch status to a clearer operations surface. Compass already has the first cockpit implementation and should receive QA polish after the shared direction stabilizes.

## Verification Plan For This Audit

- Whitespace check on this document.
- Secret-like value scan on this document.
- Confirm staged files, if any, are limited to this audit document before commit.

## No-Touch Confirmation

This Gate did not perform:

- Product code implementation.
- SQL execution.
- DB/Auth mutation.
- Production API calls.
- n8n import, activation, or execution.
- Secret/env/token/cookie/session/credential readback.
- External asset import.
