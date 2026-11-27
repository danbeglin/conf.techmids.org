---

description: "Task list template for feature implementation"
---

# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`

**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: The examples below include test tasks. Tests are OPTIONAL - only include them if explicitly requested in the feature specification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/`, `tests/` at repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` or `android/src/`
- Paths shown below assume single project - adjust based on plan.md structure

<!--
  ============================================================================
  IMPORTANT: The tasks below are SAMPLE TASKS for illustration purposes only.

  The /speckit-tasks command MUST replace these with actual tasks based on:
  - User stories from spec.md (with their priorities P1, P2, P3...)
  - Feature requirements from plan.md
  - Entities from data-model.md
  - Endpoints from contracts/

  Tasks MUST be organized by user story so each story can be:
  - Implemented independently
  - Tested independently
  - Delivered as an MVP increment

  DO NOT keep these sample tasks in the generated tasks.md file.
  ============================================================================
-->

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization, strict typing, and design token setup

- [ ] T001 Verify/create project directory structure per implementation plan
- [ ] T002 Configure strict static typing and testing harness per plan.md
- [ ] T003 [P] Register required design tokens/CSS variables in `tailwind.config.js` or `src/styles/` (zero magic values)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Shared types, data contracts, and base primitives that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

Examples of foundational tasks (adjust based on your project):

- [ ] T004 Define strict static TypeScript interfaces/types for props, events, and state models in `src/types/`
- [ ] T005 [P] Create shared pure UI primitives (e.g., loading/empty/error state wrappers, accessible focusable controls) in `src/components/`
- [ ] T006 [P] Setup data-fetching or static data adapters decoupled from presentation components

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - [Title] (Priority: P1) 🎯 MVP

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### User-Centric Tests for User Story 1 (include when tests are in scope) ⚠️

> **NOTE: Write user-centric behavior tests FIRST (rendering, clicks, keyboard navigation, form validation), ensure they FAIL before implementation, and avoid testing internal implementation details**

- [ ] T010 [P] [US1] Component behavior test (states, clicks, keyboard focus/ARIA) in `tests/components/[Component].test.tsx`
- [ ] T011 [P] [US1] End-to-end user journey test across mobile/desktop viewports in `tests/e2e/[journey].spec.ts`

### Implementation for User Story 1

- [ ] T012 [P] [US1] Build pure, strictly-typed presentational component(s) using semantic HTML and theme tokens in `src/components/[Component].tsx`
- [ ] T013 [P] [US1] Implement mobile-first responsive layout (Flexbox/Grid + relative units) across mobile, tablet, and desktop
- [ ] T014 [US1] Wire state/data container and explicit loading, empty, error, and success states in `src/pages/[page].tsx` (depends on T012)
- [ ] T015 [US1] Add immediate visual interaction feedback (disabled/spinner/optimistic state) and verify keyboard navigation + `aria-label`/`alt` attributes

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - [Title] (Priority: P2)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### User-Centric Tests for User Story 2 (include when tests are in scope) ⚠️

- [ ] T018 [P] [US2] Component behavior test in `tests/components/[Component].test.tsx`
- [ ] T019 [P] [US2] End-to-end user journey test in `tests/e2e/[journey].spec.ts`

### Implementation for User Story 2

- [ ] T020 [P] [US2] Build pure, strictly-typed UI component(s) using semantic HTML and theme tokens in `src/components/[Component].tsx`
- [ ] T021 [US2] Implement mobile-first responsive styles and explicit UI states (`loading`, `empty`, `error`, `success`)
- [ ] T022 [US2] Wire interactions and immediate visual feedback in `src/pages/[page].tsx`
- [ ] T023 [US2] Integrate with User Story 1 components while preserving independent testability

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - [Title] (Priority: P3)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### User-Centric Tests for User Story 3 (include when tests are in scope) ⚠️

- [ ] T024 [P] [US3] Component behavior test in `tests/components/[Component].test.tsx`
- [ ] T025 [P] [US3] End-to-end user journey test in `tests/e2e/[journey].spec.ts`

### Implementation for User Story 3

- [ ] T026 [P] [US3] Build pure, strictly-typed UI component(s) using semantic HTML and theme tokens in `src/components/[Component].tsx`
- [ ] T027 [US3] Implement mobile-first responsive styles and explicit UI states (`loading`, `empty`, `error`, `success`)
- [ ] T028 [US3] Wire interactions and immediate visual feedback in `src/pages/[page].tsx`

**Checkpoint**: All user stories should now be independently functional

---

[Add more user story phases as needed, following the same pattern]

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Accessibility, visual consistency, and performance checks across stories

- [ ] TXXX [P] Audit WCAG 2.1 AA contrast ratios, semantic landmarks, `aria-label`/`alt` text, and keyboard focus rings
- [ ] TXXX [P] Audit components for zero hardcoded hex colors, pixel spacing, or untyped `any` props
- [ ] TXXX Verify responsive mobile-first layouts across mobile, tablet, and desktop viewports
- [ ] TXXX Run `npm run lint`, type-checking, and `quickstart.md` validation

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - May integrate with US1 but should be independently testable
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - May integrate with US1/US2 but should be independently testable

### Within Each User Story

- Tests (if included) MUST be written and FAIL before implementation
- Models before services
- Services before endpoints
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Launch all tests for User Story 1 together (if tests requested):
Task: "Contract test for [endpoint] in tests/contract/test_[name].py"
Task: "Integration test for [user journey] in tests/integration/test_[name].py"

# Launch all models for User Story 1 together:
Task: "Create [Entity1] model in src/models/[entity1].py"
Task: "Create [Entity2] model in src/models/[entity2].py"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence
