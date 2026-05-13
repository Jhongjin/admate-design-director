# Design Director Compass Cockpit Foundation Plan v1

Date: 2026-05-13
Gate: Design-Director-Compass-Cockpit-Foundation-1
Status: Draft, docs-only
Owner: Design Director / Commander review

## Scope

This document defines the Compass cockpit foundation for future Compass UX work.

Allowed:

- Translate the AdMate taste strategy into Compass-specific UX direction.
- Define evidence workspace behavior for policy/RAG answers.
- Prepare implementation handoff guidance.

Not allowed:

- Compass repo code changes.
- RAG, API, DB, crawler, reembedding, noData, or prompt logic changes.
- Production smoke, SQL, n8n execution, Auth/DB mutation.
- Secret, token, cookie, session, credential, or environment value readback.

## Product Role

Compass is the policy evidence workspace.

It should help operators ask advertising policy questions, inspect answer confidence, and understand source basis without feeling like a generic chatbot.

## Audience

Primary:

- Media planners.
- Policy reviewers.
- Internal operators checking platform rules.

Secondary:

- Product owners evaluating answer quality and source coverage.

## First Viewport Contract

The Compass first viewport must answer:

1. What question is being evaluated?
2. What answer state exists: answered, generation-limited, noData, degraded, or pending?
3. Which sources support the answer?
4. What should the user do next?

Required modules:

- Focused input composer.
- Answer timeline or response pane.
- Evidence/source panel.
- Answer status chips: confidence, generation-limited, noData, degraded.
- Source count and source preservation state.
- Empty-state prompt guidance that does not become marketing copy.

## Evidence Workspace Contract

The evidence panel is not secondary decoration. It is a core trust surface.

It should show:

- Source title.
- Platform or policy family.
- Snippet or summarized basis.
- Verification/source status.
- Degraded/generation-limited preservation note.

It must avoid:

- Raw DB row dumps.
- Raw embeddings.
- Internal ranking payloads.
- Full hidden prompt text.

## Answer State Contract

Answered:

- Show concise answer and visible evidence.
- Let source panel remain inspectable.

Generation-limited:

- Say that answer generation is limited.
- Preserve verified sources.
- Avoid implying no evidence exists.

noData:

- Say that no verified evidence supports the request.
- Hide unrelated generic sources.
- Offer safe next query guidance.

Degraded:

- Preserve what is known.
- Show retry or source-only mode if available.

## Visual Taste Direction

Compass should feel like:

- Legal/policy research desk.
- Evidence-first analyst workspace.
- Calm Korean-language review tool.

It should not feel like:

- Generic AI chat app.
- Decorative assistant page.
- Black-box answer generator.
- Marketing support bot.

The design can be richer through evidence hierarchy, source confidence, and review states rather than bright decorative treatment.

## Responsive Contract

Desktop:

- Answer and evidence can coexist.
- Source panel should not push the composer out of view.
- Long Korean input must wrap cleanly.

Mobile:

- Source panel should become an inspectable section or drawer.
- Composer remains reachable.
- No horizontal overflow.
- Long source titles and Korean policy text must wrap.

## Implementation Candidate Surfaces

Future implementation gate may inspect and update:

- Compass chat page.
- Evidence/source panel layout.
- Empty, noData, generation-limited, and degraded states.
- Input composer and long Korean text behavior.

No RAGSearchService, route, DB, reembedding, crawler, or noData logic changes are included unless separately approved.

## Verification Plan

- Desktop 1440x900 smoke.
- Mobile 390x844 smoke.
- Long Korean input overflow check.
- Source panel viewport containment.
- Generation-limited source preservation state.
- noData empty-source state.
- No raw prompt, DB, secret, token, cookie, session, or provider payload exposure.

## Acceptance Criteria

- Compass feels like an evidence desk, not a chatbot demo.
- The user can see why an answer exists or why it cannot be produced.
- Source preservation and noData behavior are visually distinct.
- Mobile keeps input, answer, and evidence usable.
