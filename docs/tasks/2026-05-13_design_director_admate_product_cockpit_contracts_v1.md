# Design Director AdMate Product Cockpit Contracts v1

Date: 2026-05-13
Gate: Design-Director-AdMate-Product-Cockpit-Contracts-1
Status: Draft, docs-only
Owner: Design Director / Commander review

## Scope

This document translates the AdMate product taste strategy into product-level cockpit contracts for Homepage, Compass, Sentinel, Lens, and Foresight.

This gate does not implement UI, change product repos, import external themes, generate assets, call production APIs, inspect Auth/session data, execute SQL, mutate DB/Auth state, trigger capture jobs, run n8n workflows, or read secret/environment values.

## Inputs

- `docs/tasks/2026-05-13_design_director_admate_product_taste_strategy_v1.md`
- `docs/design-system/admate-design-system-v1.md`
- `docs/product-ui-direction/compass-ui-direction-v1.md`
- `docs/product-ui-direction/sentinel-openclaw-ui-direction-v1.md`
- `docs/product-ui-direction/lens-ui-direction-v1.md`
- `docs/product-ui-direction/foresight-ui-direction-v1.md`
- Read-only product contract drafts from Homepage, Compass, Sentinel, Lens, and Foresight review agents.

## Shared Cockpit Contract

Each AdMate product main page should be a cockpit, not a marketing landing page.

The first viewport must answer four questions:

1. What product am I in?
2. What is the current state?
3. What should I do next?
4. What evidence, data, or operational signal makes that state trustworthy?

The shared product cockpit should include:

- Product identity and operating mode.
- Current state summary.
- Primary next action.
- Work queue or recent activity.
- Evidence, data readiness, or source-health panel.
- Risk, blocker, degraded, or empty-state explanation.
- Account/access/session state only where it materially affects the workflow.

Shared design constraints:

- Use restrained, work-focused layouts. Avoid generic AI hero pages, oversized marketing copy, decorative gradients, blurred orbs, or purely atmospheric visuals.
- Keep cards at 8px radius or less unless the existing product system requires otherwise.
- Use product accents sparingly for state and navigation, not as a one-note palette.
- Prioritize readable Korean copy, stable responsive dimensions, and no horizontal overflow.
- Make empty and degraded states useful. Empty states should explain what is missing and what can be done next.
- Do not expose raw provider payloads, API keys, tokens, cookies, sessions, env values, DB internals, raw SQL output, raw logs, or unredacted campaign/account identifiers.

## Homepage / Command Center

Target user:
Executives, product owners, and internal stakeholders who need a read-only status overview across the AdMate product family.

Audience boundary:
Homepage may have public-safe or authenticated variants, but the executive cockpit version must use redacted, basis-backed operating status and must not expose internal write controls or raw operational data.

First viewport promise:
AdMate is an AI agent-based advertising operations platform, and Command Center shows product readiness, status, blockers, and next milestones without exposing internals.

The homepage must not read like a platform feature manual. It should feel like an executive operating room: current product confidence, decision queue, blockers, ownership, and where attention should move next.

Required panels:

- Executive summary with current reporting basis, overall progress, last updated date, and status counts.
- Workspace status card with operating health, product-signal health, governance/automation status, and next milestone.
- Product cards for Compass, Sentinel, Lens, Foresight, and Agent Core where appropriate.
- Each product card should show role, owner, status badge, progress or phase, this-week work, this-week output, blocked issue, next milestone, and target date.
- Public-safe entry links only. Internal/admin/write paths must be absent or clearly unavailable.

Homepage success is measured by whether the first viewport communicates current operating reality, ownership, blockers, and next decisions, not by how completely it explains product features.
Progress, confidence, and readiness values must show a visible basis, last-updated state, or stale/manual marker. Do not present aspirational progress as operating truth.

Empty and error states:

- Live-data unavailable should not look normal. Show read-only fallback, stale basis, and next verification path.
- Planned or review-needed product states must not look production-complete.
- Route failure must not expose debug overlays, stack traces, local URLs, env names, read keys, raw payloads, or account data.
- Product descriptions must not become the primary content. Use them only to orient the reader after state, risk, owner, and next action are visible.

Mobile expectations:

- Header, basis, summary cards, workspace status, and product cards stack vertically.
- Product name, status badge, date, owner, progress, and milestone must remain inside cards.
- Progress bars and badges must remain readable at 320-390px width.

No-touch risks:

- Do not expose internal admin actions, credentials, read keys, DB/Auth details, raw provider/API payloads, or real advertiser/campaign/account data.
- Do not overclaim product maturity without a visible basis.
- Do not present Openclaw or Hermes as external standalone products. They are Agent Core internals.

## Compass

Target user:
Media planners, policy QA reviewers, and operators who need grounded advertising policy answers with visible evidence.

First viewport promise:
Compass answers policy questions only as far as retrieved evidence supports them, and the user can immediately inspect the source basis.

Required panels:

- Answer workspace with grounded response, citation chips, generation state, and noData or generation-limited explanation.
- Source health panel showing platform coverage, freshness, parse/index status, and retrieval availability.
- Evidence/source panel with ranked sources, platform, title, document URL or safe identifier, snippet, freshness, and relevance.
- History panel with prior questions, answer state, source count, timestamp, and platform/filter context.
- Policy scope controls for platform and policy category filters.
- Multi-LLM or validation panel if the workflow is enabled, showing agreement, disagreement, and needs-human-review states.

States:

- `ready`
- `retrieving`
- `answering`
- `generation-limited`
- `noData`
- `sourceDegraded`
- `validationConflict`
- `error`

Mobile expectations:

- Answer appears first, followed by compact source-health summary.
- Sources move into a drawer or bottom sheet, opened from citation chips.
- Long URLs, snippets, and Korean platform labels must wrap without horizontal scroll.

No-touch risks:

- Do not weaken citation visibility.
- Do not blur `noData` into a generic answer.
- Do not imply certainty when evidence is missing, stale, conflicting, or generation-limited.
- Do not touch RAGSearchService, `/api/chat-ollama`, DB/import/reembedding/crawler, or noData policy in a visual-only gate.

## Sentinel

Target user:
Campaign operators, media planners on duty, and audit/reliability reviewers who need to know whether advertising operations are safe right now.

First viewport promise:
Sentinel shows operational safety within seconds: prelaunch ingest health, live monitoring status, alert severity, pending operator action, and fail-closed blockers.

Required panels:

- Safety summary with normal, attention, degraded, blocked, or fail-closed state and last verified timestamp.
- Prelaunch ingest status with latest event, read-model freshness, source workflow, duplicate flag, and row-delta/readback health.
- Live monitoring with campaign status checks, stale monitor detection, and latest normal/warn/error result.
- Alert queue grouped by severity, age, source, safe campaign label, and suppression/hold state.
- Operator actions such as acknowledge, hold, stop today, suppress, resume, and retry request, with explicit audit state.
- Audit trail with actor, timestamp, sanitized reason, and related alert/event reference.
- Delivery status for Slack/email/webhook success, skipped, suppressed, and failed states.

States:

- `normal`
- `attention`
- `degraded`
- `blocked`
- `fail-closed`
- `stale`
- `suppressed`
- `skipped`
- `pending action`
- `resolved`

Mobile expectations:

- Mobile triage order is safety summary, top blockers, active alert queue, then operator actions.
- Tables collapse into dense rows with severity, age, status, and one clear next action.
- Fail-closed reason and last verified timestamp must remain visible.

No-touch risks:

- Do not expose raw campaign IDs, provider payloads, request fingerprints, source run hashes, ingest keys, cookies, tokens, env values, storage paths, or raw SQL output.
- Do not create mutation affordances unless auth, audit logging, and confirmation gates are explicit.
- Do not treat red, amber, and green as decorative colors. They must represent operational state.

## Lens

Target user:
Capture operators, QA leads, and planners who need to monitor capture jobs, inspect visual outputs, and approve or reject evidence-quality results.

First viewport promise:
Lens shows what is capturing now, what needs review, and what is blocked.

Required panels:

- Capture queue with queued, running, succeeded, failed, canceled, and retrying jobs.
- Queue rows should show safe job identifier, source, target surface, owner, started time, duration, attempt count, and status.
- Golden sample QA panel showing baseline sample, latest output, pass/fail/needs-review, and fidelity notes.
- Preview inspection with thumbnail/grid preview and quick open.
- Detail inspection with full output view, metadata, capture parameters, dimensions, artifacts, warnings, and retry history.
- Action bar for cancel, retry, mark approved, mark rejected, and copy diagnostic summary, with confirmation around destructive actions.
- Sanitized run log summary and filters for status, source, owner, date window, failure reason, and QA verdict.

States:

- `empty`
- `loading`
- `running`
- `success`
- `needsReview`
- `failed`
- `canceled`
- `retrying`
- `stale`
- `unknown`

Mobile expectations:

- Mobile is list-first. Queue health, urgent failures, and review count come before dense inspection.
- Preview detail can open in fullscreen, with fit/zoom modes that do not distort output.
- Cancel and retry remain reachable but protected against accidental taps.

No-touch risks:

- Do not expose raw capture credentials, cookies, sessions, env vars, signed URLs, or provider payloads.
- Do not let preview status alone equal QA pass without golden criteria.
- Do not hide failed or canceled attempts from history.
- Do not alter capture rendering, injection, platform templates, output fidelity, or storage behavior in a cockpit-only gate.

## Foresight

Target user:
Media planners and performance strategists preparing a forecast before budget approval.

First viewport promise:
Foresight shows expected outcome, benchmark confidence, data coverage, and what the planner should adjust next.

Required panels:

- Forecast simulator controls for industry, objective, gender, age, duration, and budget.
- Primary forecast summary for reach, CPM, CPC, link CPC, CPV, VTR, frequency, or a clear empty/loading state.
- Benchmark and data-readiness panel showing source coverage, matched count, industry selection, and benchmark state.
- Confidence/coverage labels near every benchmarked KPI.
- Optimization guidance with scenario comparison, target expansion, saturation, seasonality, and data-quality warnings.
- Budget curve and scenario panels with explicit loading and empty states.
- ML comparison panel that is visually separate from baseline forecast.
- Account/session state through existing navigation and login redirect behavior.

States:

- `preRun`
- `loading`
- `emptyFilters`
- `noBenchmarkData`
- `lowConfidence`
- `validationError`
- `securityReview`
- `authExpired`
- `exportUnavailable`

Mobile expectations:

- Single-column flow: setup, primary forecast, trust/coverage, guidance, charts.
- Run or rerun action should stay near the top.
- KPI cards may use two columns only when Korean text wraps cleanly; otherwise use one column.
- Charts need fixed height and horizontal-safe labels.

No-touch risks:

- Do not expose raw account, campaign, ad set, advertiser, token, cookie, URL, or credential markers.
- Do not make synthetic/local benchmark fixtures look production-approved.
- Do not blend ML prediction and baseline forecast into one unquestioned number.
- Do not enable export/report when confidence, validation, or security state says blocked.
- Do not change auth/session guard behavior during cockpit UI work.

## Implementation Order

Recommended cockpit rollout order:

1. Foresight cockpit foundation, because it has the clearest current empty-state gap and forecast trust surface.
2. Lens cockpit queue/detail refinement, because operators already need capture queue clarity, golden QA, and cancel/retry affordances.
3. Compass evidence workspace refinement, because source trust and generation-limited/noData distinctions are already product-critical.
4. Sentinel operator cockpit refinement, because live workflow and mutation gates require more audit caution.
5. Homepage/Command Center polish, after product cockpit semantics are stable enough to summarize accurately.

Design QA priority may still elevate Lens or Compass for specific defects. This order is for product cockpit rollout, not bug severity.

## Acceptance Criteria

- Desktop first viewport clearly shows product identity, current state, next action, and evidence basis.
- Mobile view has no horizontal overflow, clipped Korean labels, or inaccessible primary actions.
- Empty/degraded states are explicit and useful.
- State badges map to actual product state, not decorative color.
- No product repo, SQL, Auth, DB, capture, RAG, n8n, production API, or environment change is made by this contract gate.
- No external theme, template, code, asset, or vendor file is copied into product repos.

## Next Gate

Recommended next gate:

`Design-Director-Foresight-Cockpit-Foundation-Plan-1`

Scope: translate this contract into a Foresight-only implementation plan with no code changes yet.
