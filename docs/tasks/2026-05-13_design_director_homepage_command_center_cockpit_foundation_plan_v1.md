# Design Director Homepage Command Center Cockpit Foundation Plan v1

Date: 2026-05-13
Gate: Design-Director-Homepage-Command-Center-Cockpit-Foundation-1
Status: Draft, docs-only
Owner: Design Director / Commander review

## Scope

This document defines the Homepage / Command Center cockpit foundation for AdMate.

Allowed:

- Translate the product taste strategy into Homepage implementation direction.
- Define first viewport, status, redaction, responsive, and verification contracts.
- Prepare a handoff-ready plan for a future Homepage implementation gate.

Not allowed:

- Product repo code changes.
- External template, code, asset, vendor, or theme import.
- Production smoke, API calls, SQL, n8n execution, Auth/DB mutation.
- Secret, token, cookie, session, credential, or environment value readback.

## Product Role

Homepage is not a marketing explainer and not a feature catalog.

It should behave like an executive operating cockpit for AdMate: a read-only room where leadership can understand product readiness, current risk, ownership, blockers, and next decisions within the first viewport.

## Audience

Primary:

- Founder/operator.
- Product owners.
- Internal stakeholders who need status without opening every product.

Secondary:

- Future clients or partners viewing a public-safe version.

The public-safe version may show redacted status and product direction, but it must not expose internal write controls, raw logs, raw account identifiers, internal campaign names, secrets, or operational payloads.

Public-safe fields should be limited to product role, broad readiness state, redacted milestone language, and non-sensitive next-step labels. Authenticated executive fields may add owner, blocker, decision queue, stale/live basis, and internal target dates, but still must not expose raw operational payloads.

## First Viewport Contract

The first viewport must answer:

1. What is AdMate operating today?
2. Which products are healthy, blocked, or under review?
3. What decision or action needs attention next?
4. What evidence or reporting basis makes the status trustworthy?

Required first viewport modules:

- Executive headline: current operating reality, not a slogan.
- Reporting basis: date, source, stale/manual/live marker.
- Product readiness strip: Compass, Sentinel, Lens, Foresight, Agent Core.
- Decision queue: one to three next decisions with owner and status.
- Blocker/risk summary: visible when non-empty.
- Primary navigation into product cockpits.

## Information Architecture

Recommended page sections:

- Operating Overview: progress, readiness, review count, blocked count, stale count.
- Product Family Status: compact product cards with role, owner, progress basis, this-week work, next milestone.
- Decision Queue: items that require executive attention.
- Evidence Ledger: source of status, last reviewed date, and verification note.
- Product Entry: public-safe links or authenticated entry depending on route context.

Avoid:

- Long feature lists.
- “AI platform” hero claims without operational status.
- Decorative screenshots that do not reveal current product state.
- Generic purple/dark SaaS landing page composition.

## Visual Taste Direction

Homepage should feel like:

- Executive operating room.
- Calm board deck.
- Read-only status console.

It should not feel like:

- Product brochure.
- Startup landing page.
- Dense admin table.
- Decorative AI showcase.

Use restrained contrast, clear hierarchy, and product accents only for state and routing. The page can be visually richer than the current baseline, but richness should come from operating signals, not decoration.

## Responsive Contract

Desktop:

- Keep first viewport scannable without requiring deep scroll.
- Product cards should support comparison.
- Decision queue should remain visible near the status summary.

Mobile:

- Collapse to a single decision-first flow.
- Keep reporting basis and stale/manual markers visible.
- Product cards stack with stable dimensions.
- Avoid horizontal overflow and tiny dense tables.

## Empty, Stale, And Degraded States

Empty state:

- Explain what signal is missing.
- Show the next verification path.
- Do not show “normal” if data is stale or unavailable.

Stale state:

- Include last updated date.
- Mark status as stale/manual/fallback.
- Avoid showing aspirational progress as operating truth.

Degraded state:

- Show what is still available.
- Show owner and next check.
- Keep raw logs and internal payloads hidden.

## Implementation Candidate Surfaces

Future implementation gate may inspect and update:

- Homepage route and Command Center route.
- Static/read-only product status model shaping.
- Product card components.
- Responsive layout CSS.

Live data-source integration, API/DB wiring, Auth/session changes, product route changes, and secret handling require separate explicit approval. Implementation must preserve existing auth, DB, product route, and secret boundaries unless that approval exists.

## Verification Plan

Required before implementation approval:

- Desktop 1440x900 screenshot.
- Mobile 390x844 screenshot.
- No horizontal overflow.
- First viewport communicates operating reality, ownership, blockers, and next decisions.
- Public-safe route does not expose internal data.
- Stale/manual/live basis visible.

## Acceptance Criteria

- The page feels like a decision cockpit, not a feature manual.
- Every status number has basis or stale/manual marker.
- Product cards reveal current operating state, not only capabilities.
- Mobile preserves the decision queue and product status meaning.
- No secret, raw account, raw campaign, SQL, Auth, or internal payload exposure.
