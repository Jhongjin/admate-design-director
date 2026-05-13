# Design Director Sentinel Cockpit Foundation Plan v1

Date: 2026-05-13
Gate: Design-Director-Sentinel-Cockpit-Foundation-1
Status: Draft, docs-only
Owner: Design Director / Commander review

## Scope

This document defines the Sentinel cockpit foundation for future product UI work.

Allowed:

- Translate the AdMate taste strategy into Sentinel-specific UX direction.
- Define live monitoring, campaign status, workflow, ingest, and operator action surfaces.
- Prepare implementation handoff guidance.

Not allowed:

- Sentinel or Agent Core repo code changes.
- n8n import, activation, execution, credential wiring, or workflow mutation.
- Production API calls, ingest requests, SQL, DB/Auth mutation.
- Secret, token, cookie, session, credential, or environment value readback.

## Product Role

Sentinel is the operating console for campaign monitoring and controlled operational signals.

It should feel like a command console where operators can understand system health, workflow state, campaign alerts, prelaunch ingest status, and safe next actions without opening raw logs.

## Audience

Primary:

- Campaign operators.
- Admins and super admins.
- Monitoring owners.

Secondary:

- Product owners validating operational readiness.

## First Viewport Contract

The Sentinel first viewport must answer:

1. What is the current operating status?
2. Which campaigns, alerts, or workflows need attention?
3. Which ingest or automation signals are healthy?
4. What can the operator safely do next?

Required modules:

- Access/account state summary.
- Campaign monitoring counts.
- Alert health and recent alerts.
- Workflow health summary.
- Prelaunch ingest status card.
- Operator action shortcuts with permission-aware states.

## Monitoring Surface Contract

Monitoring cards should expose:

- Total monitored campaigns.
- Normal, warning, critical, resolved counts.
- Recent alert summaries.
- Last updated time and stale marker.
- Owner or escalation path.

Do not expose raw provider payloads, raw campaign IDs, full internal logs, credentials, or unredacted account data.

## Workflow And n8n Representation

n8n workflows should be represented as operational state, not raw workflow internals.

Show:

- Workflow name.
- Published/inactive/draft state.
- Last run status.
- Next run or manual-only marker.
- Producer branch enabled/disabled state where safe.
- Credential configured state without showing values.

Avoid:

- Raw execution logs.
- Credential names that imply secret values beyond approved labels.
- Node-level debugging data in the main cockpit.

## Prelaunch Ingest Card

The prelaunch ingest card should show:

- Project key.
- Environment.
- Check type.
- Latest status and severity.
- Latest received time.
- Finding count.
- Duplicate/idempotency state when relevant.
- Read-model freshness.

It should not expose raw finding payloads or raw request fingerprints.

## Visual Taste Direction

Sentinel should feel like:

- Operations command console.
- Monitoring bridge.
- Audit-aware control room.

It should not feel like:

- Generic admin dashboard.
- Marketing product page.
- Raw n8n canvas clone.
- Dense database viewer.

Use compact, high-confidence state components. Color should communicate severity and readiness, not decoration.

## Responsive Contract

Desktop:

- Monitoring summary, workflow health, and recent alerts should be visible together.
- Operator actions should be permission-aware and scannable.

Mobile:

- Prioritize current status, alerts, and safe actions.
- Collapse workflow details into expandable sections.
- Avoid horizontal tables.

## Empty, Failed, And Degraded States

No alerts:

- Show normal state and last verification time.

Workflow unavailable:

- Mark workflow state unknown/degraded.
- Show next verification path.

Ingest unavailable:

- Preserve last known status with stale marker.
- Do not show raw API errors on the main cockpit.

## Implementation Candidate Surfaces

Future implementation gate may inspect and update:

- Sentinel dashboard route.
- Live monitoring summary components.
- Prelaunch ingest status card.
- Workflow health representation.
- Permission-aware operator action cards.

Workflow activation, producer execution, SQL, API calls, and credential wiring require separate explicit approval.

## Verification Plan

- Desktop 1440x900 smoke.
- Mobile 390x844 smoke.
- Authenticated role surface check.
- No horizontal overflow.
- Prelaunch ingest card visible when data exists.
- Workflow states do not reveal secrets or raw execution logs.
- Operator actions are permission-aware.

## Acceptance Criteria

- Sentinel feels like an operational command console.
- The next operator action is clear.
- Workflow and ingest state are represented safely.
- The main page does not become a raw admin/log viewer.
