<!--
Sync Impact Report
- Version change: N/A (template) -> 1.0.0
- Modified principles:
  - [PRINCIPLE_1_NAME] -> I. UI Architecture & Component Design
  - [PRINCIPLE_2_NAME] -> II. User Experience & Visual Consistency
  - [PRINCIPLE_3_NAME] -> III. Accessibility (a11y) (NON-NEGOTIABLE)
  - [PRINCIPLE_4_NAME] -> IV. Code Quality & Testing
- Added sections:
  - Technical & Design Constraints
  - Specification, Planning & Quality Gates
- Removed sections:
  - Placeholder 5th principle slot ([PRINCIPLE_5_NAME]) — consolidated into the 4 governing principles specified for this project
- Templates requiring updates:
  - ✅ updated: .specify/templates/plan-template.md
  - ✅ updated: .specify/templates/spec-template.md
  - ✅ updated: .specify/templates/tasks-template.md
  - ✅ verified (no changes needed): .specify/templates/checklist-template.md
  - ✅ verified (no changes needed): AGENTS.md, README.md
- Follow-up TODOs: None
-->

# TechMids Conf Constitution

## Core Principles

### I. UI Architecture & Component Design

- **Component Modularity**: Engineers MUST break the interface into small, single-responsibility,
  reusable UI components. Presentation components MUST remain pure and decoupled from data-fetching
  or stateful business logic.
- **Simplicity First (YAGNI)**: Features MUST use native browser APIs, semantic HTML, and
  lightweight styling before introducing external UI libraries or state management packages. Any
  new third-party runtime dependency MUST be explicitly justified against native alternatives.
- **Responsive Mobile-First Layout**: Every view MUST render cleanly without horizontal overflow
  across mobile, tablet, and desktop viewports using relative units (`rem`, `%`, viewport units)
  and modern CSS layout primitives (Flexbox and CSS Grid).

**Rationale**: Modular, zero-bloat components keep the conference website fast to download and
render over congested venue mobile networks while remaining straightforward for contributors to
maintain.

### II. User Experience & Visual Consistency

- **Design Tokens Only**: Components MUST NOT hardcode magic hex values, arbitrary pixel spacing,
  or ad-hoc font sizes. All visual styling MUST reference centralized CSS custom properties or
  configured theme tokens (`tailwind.config.js`).
- **Explicit UI States**: Every interactive or asynchronous view MUST explicitly design for and
  render loading, empty, error, and success states. Views MUST NOT leave users staring at a blank
  or broken screen under any data condition.
- **Instant Feedback**: Every user interaction (clicks, form submissions, filters, toggles) MUST
  provide immediate visual feedback such as disabled/busy states, inline indicators, spinners, or
  optimistic UI updates.

**Rationale**: Centralized tokens preserve visual cohesion across conference editions, while
explicit state handling and instant feedback keep attendees informed when network or data conditions
vary.

### III. Accessibility (a11y) (NON-NEGOTIABLE)

- **Semantic HTML & Keyboard Navigation**: Components MUST use appropriate semantic HTML elements
  (`<button>`, `<nav>`, `<main>`, `<section>`, `<label>`) rather than generic clickable containers.
  All interactive controls MUST be operable via keyboard alone (`Tab`, `Shift+Tab`, `Enter`,
  `Space`, `Escape`) and display unmistakable visible focus indicators.
- **ARIA & Contrast**: All text and interactive UI elements MUST meet or exceed WCAG 2.1 Level AA
  color contrast ratios (4.5:1 for normal text, 3:1 for large text and UI components). All
  icon-only controls, form inputs, and non-decorative media MUST include descriptive `aria-label`,
  associated `<label>`, or `alt` attributes.

**Rationale**: TechMids Conf serves an inclusive technology community; accessibility is a baseline
functional requirement, never an optional polish pass.

### IV. Code Quality & Testing

- **Strict Typing**: All new and modified component props, custom events, hooks, and state models
  MUST enforce strict static typing (TypeScript strict mode or strictly typed contracts during
  incremental migration of legacy JavaScript files) with zero implicit `any` escapes.
- **User-Centric Testing**: Automated test suites MUST prioritize component and end-to-end tests
  that verify user-visible behavior—rendered output, keyboard and pointer interactions, explicit UI
  states, and form validation—over internal state snapshots or implementation details.

**Rationale**: Strict static types catch prop and data-shape mismatches at build time, while
behavior-driven tests protect real attendee journeys without breaking during internal refactors.

## Technical & Design Constraints

- **Frontend Stack Alignment**: Implementations MUST build idiomatic Next.js, React, Tailwind CSS,
  and Headless UI solutions aligned with the repository architecture (`src/components/`,
  `src/pages/`, `src/styles/`, `tailwind.config.js`).
- **Token Centralization**: New colors, typography scales, spacing tokens, or elevation shadows
  MUST be registered in `tailwind.config.js` or global CSS variables before consumption in
  components.
- **Asset & Bundle Discipline**: Media assets MUST declare explicit dimensions or aspect ratios to
  prevent Cumulative Layout Shift (CLS), and client-side JavaScript bundles MUST avoid heavy
  libraries when CSS or native DOM APIs suffice.

## Specification, Planning & Quality Gates

- **Specification Gate (`/speckit-specify`)**: Every feature specification MUST document target
  responsive behavior (mobile, tablet, desktop), explicit UI states (loading, empty, error,
  success), immediate interaction feedback, and keyboard/screen-reader accessibility acceptance
  criteria.
- **Planning Gate (`/speckit-plan`)**: Every implementation plan MUST pass the **Constitution
  Check** against Principles I–IV prior to Phase 0 research and again after Phase 1 design. Any
  proposed external dependency or architectural complexity MUST be justified in the plan's
  Complexity Tracking table.
- **Task & Implementation Gate (`/speckit-tasks` & `/speckit-implement`)**: Task breakdowns MUST
  explicitly account for theme token configuration, pure presentational component boundaries, strict
  type definitions, explicit UI state handling, WCAG AA / keyboard verification, and user-centric
  tests where automated testing is in scope.

## Governance

This Constitution supersedes all informal conventions and conflicting template defaults within the
repository. Every feature specification, technical plan, task breakdown, and code review MUST
verify compliance with Principles I–IV.

- **Amendment Procedure**: Amendments MUST be proposed through `/speckit-constitution` or a
  dedicated pull request updating `.specify/memory/constitution.md`, accompanied by a clear
  rationale and synchronized updates to any affected templates under `.specify/templates/`.
- **Versioning Policy**: This document follows Semantic Versioning (`MAJOR.MINOR.PATCH`):
  - `MAJOR`: Backward-incompatible governance changes, principle removals, or narrows/redefinitions
    of existing non-negotiable rules.
  - `MINOR`: Addition of a new principle or governance section, or materially expanded mandatory
    guidance.
  - `PATCH`: Wording clarifications, formatting improvements, or typo fixes that do not alter
    normative requirements.
- **Compliance Review**: Reviewers and automated Spec Kit workflows (`/speckit-plan`,
  `/speckit-analyze`, `/speckit-checklist`) MUST treat Constitution violations as blocking issues
  unless an explicit, time-bounded exception is approved and recorded in the feature plan's
  Complexity Tracking section. Runtime agent guidance is maintained in `AGENTS.md`.

**Version**: 1.0.0 | **Ratified**: 2026-11-27 | **Last Amended**: 2026-11-27
