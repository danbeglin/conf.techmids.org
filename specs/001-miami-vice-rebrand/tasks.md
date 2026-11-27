# Tasks: 80s Miami Vice / New Wave Conference Rebrand & Visual Overhaul

**Input**: Design documents from `/specs/001-miami-vice-rebrand/`

**Prerequisites**: `plan.md` (required), `spec.md` (required for user stories), `research.md`, `data-model.md`, `contracts/`, `quickstart.md`

**Tests**: Included per `plan.md` and the TechMids Conf Constitution (`Principle IV: User-Centric Testing`) using Node.js's built-in `node:test` runner (`tests/*.test.mjs`) with zero external dependencies.

**Organization**: Tasks are grouped by user story (`US1`–`US4`) in priority order so each story can be implemented, verified, and demonstrated as an independent increment.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies on incomplete tasks in the same phase)
- **[Story]**: Which user story this task belongs to (`[US1]`, `[US2]`, `[US3]`, `[US4]`)
- Exact repository file paths are included in every task description

## Path Conventions

- **Root data & config**: `event.js`, `speakers.js`, `schedule.js`, `tailwind.config.js`, `package.json`
- **Static assets**: `public/assets/`, `public/speakers/`
- **Application source**: `src/types/`, `src/styles/`, `src/components/`, `src/pages/`, `src/picks/`
- **Unpublished preview (read-only visual boundary)**: `src/new/`, `src/pages/new/`
- **Automated verification suites**: `tests/`

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Initialize directories, zero-dependency test script, and centralized root design-token architecture

- [ ] T001 Create `public/assets/`, `src/types/`, and `tests/` directories and register `"test": "node --test tests/**/*.test.mjs"` in `package.json`
- [ ] T002 [P] Author centralized `:root, html[data-theme="miami-night"]` (default) and `html[data-theme="pastel-day"]` CSS custom properties (canvas/surface/elevated/header backgrounds, primary/muted inks, hot magenta & electric cyan accents, `--mv-focus-ring`, `--mv-screen-{1,2,3}-{border,text,glow}`, `--mv-headliner-{bg,ink}`, `--mv-vcr-{bg,digit,bezel-shadow}`, sunset stops, grid lines, and duotone channels) in `src/styles/tokens.css`
- [ ] T003 [P] Map semantic CSS custom property colors, neon box-shadow utilities, and remapped `brand`/`techmids` token references in `tailwind.config.js`
- [ ] T004 Import `./tokens.css` at the top of `src/styles/tailwind.css` and configure base `html`/`body` surface/ink token bindings plus global `@media (prefers-reduced-motion: reduce)` animation/transition reset rules in `src/styles/tailwind.css`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Shared strict type contracts, core event identity constants, and legacy hardcoded-hex cleanup in shared primitives that MUST complete before user story work begins

**⚠️ CRITICAL**: No user story implementation can begin until this phase is complete

- [ ] T005 Define strict static TypeScript contracts (`ThemeId`, `ScreenId`, `SessionType`, `StaticPortraitImport`, `ScheduleSlotAssignment`, `SpeakerProfile`, `SessionsMap`, `ScheduleSlot`, `TrackSchedule`, `ScheduleMap`, `EventIdentity`, and UI component prop interfaces) with zero implicit `any` in `src/types/conference.d.ts`
- [ ] T006 [P] Update `EVENT` identity constants (`title: "Dan Beglins Mega Brum Tech Fest"`, `tagline: "Better than tech mids"`, `organizerName: "Dan Beglins Mega Brum Tech Fest"`, and `edition` getter) with `@ts-check` JSDoc typing in `event.js`
- [ ] T007 [P] Purge legacy hardcoded `#hex` literals (`#ff80b5`, `#9089fc`, `#23FFF3`) in favor of semantic CSS token references in `src/components/Testimonials.jsx` and `src/components/SharingCard.jsx` (preserving historical dated attendee quote attributions in `src/components/Testimonials.jsx`)
- [ ] T008 [P] Migrate shared supporting UI components (`src/components/Button.jsx`, `src/components/ButtonCTA.jsx`, `src/components/Sponsors.jsx`, `src/components/SponsorTiers.jsx`, `src/components/SponsorUsCTA.jsx`, `src/components/Meetups.jsx`, and `src/components/VenueInfo.jsx`) from legacy `dark:` / fixed palette classes to centralized theme tokens

**Checkpoint**: Foundation ready — token infrastructure, strict types, and `EVENT` constants are in place; user story implementation can proceed

---

## Phase 3: User Story 1 - Experience Rebranded Event & Synthwave Hero Identity (Priority: P1) 🎯 MVP

**Goal**: Rebrand every current-edition touchpoint on `/` and its canonical subpages to **Dan Beglins Mega Brum Tech Fest** and showcase the hero tagline **"Better than tech mids"** above a pure CSS horizontal-striped sunset and 3D perspective grid floor while preserving historical titles in Past Events.

**Independent Test**: Build and open `/`, `/schedule`, `/speakers`, `/speakers/DanBeglin`, `/code-of-conduct`, `/past-events`, and `/how-to-convince-your-boss`; verify header, hero, schedule welcome banner, footer, and page metadata display `"Dan Beglins Mega Brum Tech Fest"` with zero current-edition `"TechMids Conf"` strings, and verify the homepage hero renders `"Better than tech mids"` above the striped sunset and perspective grid floor without horizontal overflow from `320px` to `1440px+`.

### User-Centric Tests for User Story 1 ⚠️

> **NOTE: Write these user-centric static export assertions FIRST and confirm they FAIL before implementing User Story 1**

- [ ] T009 [US1] Create static export branding & hero test suite verifying `"Dan Beglins Mega Brum Tech Fest"` across canonical routes, `"Better than tech mids"` hero tagline, `.mv-sunset` / `.mv-grid-floor` backdrop markup, schedule welcome banner text, zero current-edition `"TechMids Conf"` strings, and preserved Past Events archive titles in `tests/export-rebrand.test.mjs`

### Implementation for User Story 1

- [ ] T010 [P] [US1] Add pure CSS `.mv-sunset` (horizontal-striped circular gradient with `-webkit-mask-image` / `mask-image` bands and horizon bloom) and `.mv-grid-floor` (clipped `perspective()` + `rotateX()` 3D grid plane gated by `prefers-reduced-motion`) component rules in `src/styles/tailwind.css`
- [ ] T011 [P] [US1] Replace legacy yellow/black wordmarks with token-driven synthwave **Dan Beglins Mega Brum Tech Fest** SVG graphics (zero hardcoded `#hex` or inline fill hexes) in `src/components/Logo.jsx` and `src/components/LogoConf.jsx`
- [ ] T012 [P] [US1] Build pure `aria-hidden="true"` presentational striped-sunset and perspective-grid stage component (`@ts-check`) in `src/components/SynthwaveBackdrop.jsx`
- [ ] T013 [P] [US1] Rebrand header banner (`EVENT.title`), footer organizer credit & copyright line, OpenGraph `og:site_name`, and Schema.org `Event` JSON-LD using `EVENT` constants and theme tokens in `src/components/Header.jsx`, `src/components/Footer.jsx`, `src/components/PageMeta.jsx`, and `src/components/EventStructuredData.jsx`
- [ ] T014 [P] [US1] Rebrand current-event title/staff references in `src/components/CodeOfConduct.jsx` and migrate decorative SVGs/surfaces to theme tokens in `src/components/CodeOfConduct.jsx` and `src/components/PastEventsList.jsx` (retaining historical archive titles in `PastEventsList.jsx`)
- [ ] T015 [P] [US1] Update opening `welcome` slot description to `'Welcome to Dan Beglins Mega Brum Tech Fest!'` in `schedule.js`
- [ ] T016 [US1] Refactor homepage hero to compose `SynthwaveBackdrop`, `LogoConf`, single semantic `<h1>` with `"Dan Beglins Mega Brum Tech Fest"` and `"Better than tech mids"`, rebranded intro copy, CLS-safe fixed-dimension media, and overflow-free mobile-first (`320px+`) layout in `src/components/Hero.jsx`
- [ ] T017 [US1] Update canonical page `<PageMeta>` titles/descriptions, semantic `<main>` surfaces, and current-event business-case copy across `src/pages/index.jsx`, `src/pages/schedule.jsx`, `src/pages/speakers/index.jsx`, `src/pages/speakers/[speaker].jsx`, `src/pages/code-of-conduct.jsx`, `src/pages/past-events.jsx`, `src/pages/how-to-convince-your-boss.jsx`, and `src/pages/sharing/index.jsx`

**Checkpoint**: User Story 1 is complete and independently testable as an MVP increment

---

## Phase 4: User Story 2 - Switch Between Miami Night and Pastel Day Themes (Priority: P1)

**Goal**: Load every first-time visit directly into **Miami Night** (`data-theme="miami-night"`) with zero wrong-theme flash regardless of OS color scheme, provide an accessible **Miami Night / Pastel Day** header switcher (`<100ms` toggle + `localStorage` persistence with `try/catch` fallback), and guarantee WCAG 2.1 Level AA contrast across both palettes.

**Independent Test**: Open `/` in a fresh session (with OS in Light or Dark mode) to confirm `data-theme="miami-night"` paints immediately; focus the header theme switcher via `Tab` and toggle with `Enter`/`Space` or click; verify visible `--mv-focus-ring`, updated `aria-pressed` / `aria-label`, immediate transition to `pastel-day`, zero FOUC across reloads/navigation, and passing contrast tests.

### User-Centric Tests for User Story 2 ⚠️

> **NOTE: Write these contrast and theme-bootstrap tests FIRST and confirm missing pieces FAIL before implementing User Story 2**

- [ ] T018 [P] [US2] Create automated WCAG 2.1 Level AA relative-luminance contrast test (`>= 4.5:1` normal text, `>= 3.0:1` UI borders/focus rings across both `miami-night` and `pastel-day` in `src/styles/tokens.css`) and zero-hardcoded-`#hex` scanner (`src/components/**`, `src/pages/**`) in `tests/tokens-contrast.test.mjs`
- [ ] T019 [P] [US2] Extend static export test suite with assertions for SSR `<html lang="en" data-theme="miami-night"`, blocking `<head>` `localStorage` `data-theme` bootstrapper script, and accessible `ThemeSwitcher` button attributes in `tests/export-rebrand.test.mjs`

### Implementation for User Story 2

- [ ] T020 [P] [US2] Update `<Html lang="en" data-theme="miami-night" className="antialiased">`, token-driven `<body>` classes, and synchronous `try/catch`-guarded `<Head>` `data-theme` bootstrapper script in `src/pages/_document.jsx`
- [ ] T021 [P] [US2] Build pure, strictly-typed `Miami Night / Pastel Day` switcher component (`<button type="button">`, `aria-pressed`, descriptive `aria-label`, `2px` `--mv-focus-ring`, synchronous `document.documentElement.dataset.theme` swap, and `try/catch` `localStorage` persistence) in `src/components/ThemeSwitcher.jsx`
- [ ] T022 [US2] Replace legacy inline `ThemeToggle` with `ThemeSwitcher` inside `<nav aria-label="Main">` in `src/components/Header.jsx`

**Checkpoint**: User Stories 1 AND 2 are both functional and independently verifiable with 100% WCAG AA token compliance

---

## Phase 5: User Story 3 - Browse Updated Speaker Lineup & 80s Headliner Cards (Priority: P2)

**Goal**: Deliver the updated 24-speaker lineup—promoting **Dan Beglin** to Keynote (`Screen 3, 09:45–10:30`) with a glowing neon **`HEADLINER`** badge, adding **Lucia Caminos & Jason Duval** (`Screen 2, 11:15–12:00`) and **Ken Rosenberg** (`Screen 1, 16:30–17:15`) with custom `800×800` full-color 80s retro SVG portraits, and removing Simon Stevens and Mikolaj Maciejak—rendered on 80s duotone cards that snap to full natural color on pointer hover and keyboard focus.

**Independent Test**: Browse `/#speakers`, `/speakers`, `/speakers/DanBeglin`, `/speakers/LuciaCaminosJasonDuval`, and `/speakers/KenRosenberg`; verify exact speaker roles/titles, `HEADLINER` badge on Dan Beglin only, resting magenta/cyan duotone portraits snapping to full color on `:hover` and keyboard `Tab` focus, monogram fallback handling, and intact `/new` data compatibility.

### User-Centric Tests for User Story 3 ⚠️

> **NOTE: Write these speaker/schedule contract and static-export tests FIRST and confirm they FAIL before implementing User Story 3**

- [ ] T023 [P] [US3] Create data & asset contract test suite verifying 24 speakers, `DanBeglin` sole `Keynote` (`screen3, 09:45 AM`), `LuciaCaminosJasonDuval` (`screen2, 11:15 AM`), `KenRosenberg` (`screen1, 16:30 PM`), removal of `SimonStevens` & `MikolajMaciejak`, 24-slot bijection, `800x800` SVG portraits, and `/new` `src/new/data.js` compatibility in `tests/data-schedule.test.mjs`
- [ ] T024 [P] [US3] Extend static export test suite to verify `HEADLINER` badge exclusivity on `DanBeglin`, `.mv-duotone-frame` portrait rendering, and exported speaker detail pages (`out/speakers/DanBeglin.html`, `out/speakers/LuciaCaminosJasonDuval.html`, `out/speakers/KenRosenberg.html`) in `tests/export-rebrand.test.mjs`

### Implementation for User Story 3

- [ ] T025 [P] [US3] Author self-contained `800x800` (`viewBox="0 0 800 800"`) full-color 1980s synthwave co-speaker illustration with wide shadow-to-highlight luminance separation (`L = 0.08` to `0.90`) in `public/assets/lucia-jason.svg`
- [ ] T026 [P] [US3] Author self-contained `800x800` (`viewBox="0 0 800 800"`) full-color 1980s Miami counsel portrait illustration with wide shadow-to-highlight luminance separation in `public/assets/ken-rosenberg.svg`
- [ ] T027 [P] [US3] Add pure CSS `.mv-duotone-frame` (`isolation: isolate`, `grayscale(100%) contrast(1.15)` + `mix-blend-mode: multiply`/`screen` snapping to full color on `:hover`, `:focus-visible`, and `:focus-within`) and `.mv-badge-headliner` rules in `src/styles/tailwind.css`
- [ ] T028 [P] [US3] Build pure strictly-typed `HeadlinerBadge` component rendering neon `HEADLINER` for `sessionType === "Keynote"` (`@ts-check`) in `src/components/HeadlinerBadge.jsx`
- [ ] T029 [P] [US3] Build pure strictly-typed `SpeakerPortrait` component with fixed aspect-ratio placeholder frame, `.mv-duotone-frame` duotone-to-color styling, and stylized retro SVG monogram `onError` fallback (`@ts-check`) in `src/components/SpeakerPortrait.jsx`
- [ ] T030 [US3] Update `Sessions` in `speakers.js` to promote `DanBeglin` to Keynote (`screen3, 09:45 AM – 10:30 AM`), add `LuciaCaminosJasonDuval` (`screen2, 11:15 AM – 12:00 PM`) and `KenRosenberg` (`screen1, 16:30 PM – 17:15 PM`) via static SVG imports, remove `SimonStevens` and `MikolajMaciejak`, and assign explicit `slot` descriptors to all 24 speakers (`@ts-check`) in `speakers.js`
- [ ] T031 [US3] Refactor `schedule.js` into a deterministic 3-screen schedule builder derived from `Sessions` in `speakers.js` (preserving `OVERALL_START`, `OVERALL_END`, `conferenceSchedule`, `'Welcome to Dan Beglins Mega Brum Tech Fest!'`, zero `event.js` imports, and `/new` parser compatibility) in `schedule.js`
- [ ] T032 [P] [US3] Refactor speaker directory grid (`SpeakerPortrait`, `HeadlinerBadge`, visible `--mv-focus-ring` card links, mobile-first 1–4 column `gap` grid without horizontal overflow, clean wrapping for co-speaker names/long titles, and themed empty-lineup fallback linking to `/past-events`) in `src/components/Speakers.jsx`
- [ ] T033 [P] [US3] Refactor speaker profile detail view (`SpeakerPortrait`, `HeadlinerBadge`, scheduled screen & time window display from `speaker.slot`, semantic heading hierarchy, accessible social icon links, and centralized theme tokens) in `src/components/Speaker.jsx`

**Checkpoint**: User Story 3 is complete — all 24 speakers, SVG portraits, duotone hover/focus cards, detail pages, and derived schedule data work independently

---

## Phase 6: User Story 4 - Plan Attendance on the VHS / Arcade 3-Screen Schedule (Priority: P2)

**Goal**: Default the conference schedule to the **Combined** 3-screen view ordered **Screen 1 → Screen 2 → Screen 3** (retaining Stacked and My day toggles), apply three distinct neon border/accent colors to Screen 1, Screen 2, and Screen 3 across all views, and equip the **"My day"** picker control with an instant-updating retro **VCR LED counter** (`00/08`).

**Independent Test**: Open `/schedule` and `/#schedule`; verify Combined view opens by default ordered `Screen 1 → Screen 2 → Screen 3` with distinct neon borders per screen and all 24 talk slots populated; click `Pick` / `✓ Picked` across slots to verify immediate `VcrCounter` updates (`00/08` → `03/08`), dimmed competing options, `"My day"` empty (`00/08`), success (`"Link copied"`), and clipboard-fallback states.

### User-Centric Tests for User Story 4 ⚠️

> **NOTE: Write these schedule view, neon border, and VCR LED counter assertions FIRST and confirm they FAIL before implementing User Story 4**

- [ ] T034 [US4] Extend static export test suite to verify default Combined view rendering, strict `Screen 1 -> Screen 2 -> Screen 3` column order, distinct Screen 1–3 neon border classes, `00/08` VCR LED counter output, and `@media (prefers-reduced-motion: reduce)` CSS presence in `tests/export-rebrand.test.mjs`

### Implementation for User Story 4

- [ ] T035 [P] [US4] Add pure CSS `.mv-vcr-counter` (dark recessed bevel, subtle scanline overlay, `font-variant-numeric: tabular-nums` phosphor digit glow) and `.mv-screen-1` / `.mv-screen-2` / `.mv-screen-3` neon border, badge, and active-indicator rules in `src/styles/tailwind.css`
- [ ] T036 [P] [US4] Build pure strictly-typed `VcrCounter` component rendering zero-padded `00/08` tabular LED digits with screen-reader accessible count text (`@ts-check`) in `src/components/VcrCounter.jsx`
- [ ] T037 [US4] Refactor `src/components/Schedule.jsx` to default `view` to `'combined'` (unless `#p=`/`#picks=` URL hash selects `'myday'`), order `ROOMS` as `Screen 1 -> Screen 2 -> Screen 3`, apply `--mv-screen-{1,2,3}-*` neon borders across Combined, Stacked, and My day views, integrate `VcrCounter` and `HeadlinerBadge`, and enforce explicit empty (`00/08` prompt & disabled actions), error (inline shareable URL fallback in `role="status"`), and success (`✓ Picked` neon highlight + `"Link copied"` status) states in `src/components/Schedule.jsx`

**Checkpoint**: All four user stories (`US1`–`US4`) are independently functional and complete

---

## Phase 7: Polish & Cross-Cutting Concerns

**Purpose**: Final cross-story static typing audit, `/new` boundary verification, lint, production static export, and end-to-end automated validation

- [ ] T038 [P] Audit `@ts-check` JSDoc annotations and `src/types/conference.d.ts` bindings across `event.js`, `speakers.js`, `schedule.js`, and all modified files in `src/components/` and `src/pages/` to ensure zero implicit `any` types
- [ ] T039 [P] Verify unpublished `/new` preview isolation (`src/new/data.js`, `src/new/SpeakerCard.jsx`, `src/new/ScheduleBoard.jsx`, and `src/pages/new/`) to confirm zero visual styling changes on `/new` and intact shared speaker/schedule consumption
- [ ] T040 Execute full verification pipeline (`npm run lint`, `npm run build`, and `npm test`) to confirm clean static HTML export in `out/`, 100% WCAG 2.1 AA contrast compliance, zero hardcoded `#hex` component literals, and passing walkthrough checks per `specs/001-miami-vice-rebrand/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1: Setup (`T001`–`T004`)**: No dependencies — starts immediately.
- **Phase 2: Foundational (`T005`–`T008`)**: Depends on Phase 1 completion — **BLOCKS** all user stories.
- **Phase 3: User Story 1 (`T009`–`T017`, P1 🎯 MVP)**: Depends on Phase 2 completion.
- **Phase 4: User Story 2 (`T018`–`T022`, P1)**: Depends on Phase 2 completion (recommended immediately after `US1` since `T022` integrates `ThemeSwitcher` into the rebranded `src/components/Header.jsx` from `T013`, and `T018` verifies zero hardcoded `#hex` literals after `T011`/`T016`).
- **Phase 5: User Story 3 (`T023`–`T033`, P2)**: Depends on Phase 2 completion; independent of `US2`.
- **Phase 6: User Story 4 (`T034`–`T037`, P2)**: UI primitives (`T035`, `T036`) depend only on Phase 2; full 24-slot new-speaker schedule verification (`T034`, `T037`) integrates cleanly after `US3` (`T030`, `T031`) populates the derived 3-screen `Schedule` data.
- **Phase 7: Polish (`T038`–`T040`)**: Depends on Phases 3–6 completion.

### Dependency Graph

```text
Phase 1: Setup (T001 → T002, T003 → T004)
   │
   ▼
Phase 2: Foundational (T005 → T006, T007, T008)
   │
   ├───────────────────────────────┬───────────────────────────────┐
   ▼                               ▼                               ▼
Phase 3: US1 (P1 MVP)        Phase 5: US3 (P2)               Phase 6: US4 UI Primitives
Rebrand & Synthwave Hero     24-Speaker Data, SVG Portraits  VcrCounter & Screen 1–3 CSS
(T009 → T010..T015 → T016    & Duotone / HEADLINER Cards     (T035, T036)
 → T017)                     (T023..T029 → T030 → T031            │
   │                          → T032, T033)                       │
   ▼                               │                               │
Phase 4: US2 (P1)                  └───────────────┬───────────────┘
Miami Night / Pastel Day                           ▼
Zero-Flash Switcher & AA Audit            Phase 6: US4 Schedule Integration
(T018, T019 → T020, T021 → T022)          (T034 → T037)
   │                                               │
   └───────────────────────┬───────────────────────┘
                           ▼
                Phase 7: Polish & Full Build Gate
                (T038, T039 → T040)
```

### Within Each User Story

- User-centric test tasks MUST be written first and verified to fail prior to story implementation.
- Pure CSS rules and presentational leaf components (`SynthwaveBackdrop`, `ThemeSwitcher`, `HeadlinerBadge`, `SpeakerPortrait`, `VcrCounter`) are built before composite containers (`Hero`, `Header`, `Speakers`, `Speaker`, `Schedule`).
- Static SVG assets (`T025`, `T026`) precede `speakers.js` (`T030`), which precedes derived `schedule.js` (`T031`).

---

## Parallel Execution Examples

### User Story 1 (Rebrand & Synthwave Hero)

```bash
# After T009 fails, launch independent CSS, logo, backdrop, layout, and welcome-banner tasks together:
Task T010: "Add pure CSS .mv-sunset and .mv-grid-floor component rules in src/styles/tailwind.css"
Task T011: "Replace legacy wordmarks with token-driven synthwave SVG graphics in src/components/Logo.jsx and src/components/LogoConf.jsx"
Task T012: "Build pure aria-hidden decorative backdrop component in src/components/SynthwaveBackdrop.jsx"
Task T013: "Rebrand Header, Footer, PageMeta, and EventStructuredData in src/components/{Header,Footer,PageMeta,EventStructuredData}.jsx"
Task T014: "Update CodeOfConduct and PastEventsList token surfaces in src/components/{CodeOfConduct,PastEventsList}.jsx"
Task T015: "Update welcome slot description in schedule.js"
```

### User Story 2 (Miami Night / Pastel Day Theme Switcher)

```bash
# Launch both test suites in parallel:
Task T018: "Create WCAG 2.1 AA contrast & zero-hex scanner in tests/tokens-contrast.test.mjs"
Task T019: "Extend static export suite with theme bootstrapper assertions in tests/export-rebrand.test.mjs"

# Launch _document.jsx bootstrapper and ThemeSwitcher component in parallel:
Task T020: "Configure data-theme='miami-night' and blocking <Head> script in src/pages/_document.jsx"
Task T021: "Build accessible ThemeSwitcher component in src/components/ThemeSwitcher.jsx"
```

### User Story 3 (Speaker Lineup, SVG Portraits & Duotone Cards)

```bash
# Launch SVG illustrations, CSS duotone/badge rules, and pure UI primitives in parallel:
Task T025: "Author 800x800 co-speaker portrait in public/assets/lucia-jason.svg"
Task T026: "Author 800x800 speaker portrait in public/assets/ken-rosenberg.svg"
Task T027: "Add .mv-duotone-frame and .mv-badge-headliner rules in src/styles/tailwind.css"
Task T028: "Build HeadlinerBadge component in src/components/HeadlinerBadge.jsx"
Task T029: "Build SpeakerPortrait component in src/components/SpeakerPortrait.jsx"

# After T030 (speakers.js) and T031 (schedule.js), refactor grid and detail views in parallel:
Task T032: "Refactor speaker grid in src/components/Speakers.jsx"
Task T033: "Refactor speaker detail view in src/components/Speaker.jsx"
```

### User Story 4 (VHS / Arcade 3-Screen Schedule & VCR LED Counter)

```bash
# Launch VCR / Screen 1–3 CSS rules and pure VcrCounter component in parallel:
Task T035: "Add .mv-vcr-counter and .mv-screen-{1,2,3} rules in src/styles/tailwind.css"
Task T036: "Build VcrCounter component in src/components/VcrCounter.jsx"
```

---

## Implementation Strategy

### MVP First (User Story 1 + Default Miami Night Tokens)

1. Complete **Phase 1: Setup** (`T001`–`T004`) — establishes `miami-night` default root tokens and reduced-motion rules.
2. Complete **Phase 2: Foundational** (`T005`–`T008`) — establishes strict contracts, `EVENT` identity, and clean shared components.
3. Complete **Phase 3: User Story 1** (`T009`–`T017`) — delivers full **Dan Beglins Mega Brum Tech Fest** rebrand and `"Better than tech mids"` synthwave sunset/grid hero across all canonical routes.
4. **STOP and VALIDATE**: Run `npm run build` and verify User Story 1 independently across `320px–1440px+` viewports.

### Incremental Delivery

1. **Foundation (`T001`–`T008`)** → Root tokens, strict types, and `EVENT` constants ready.
2. **Add `US1` (`T009`–`T017`)** → Site-wide synthwave rebrand & hero live (MVP).
3. **Add `US2` (`T018`–`T022`)** → Zero-flash `Miami Night / Pastel Day` switcher + automated WCAG AA contrast gate active.
4. **Add `US3` (`T023`–`T033`)** → Updated 24-speaker lineup, `800×800` retro SVG portraits, duotone hover/focus cards, and `HEADLINER` Keynote badge live.
5. **Add `US4` (`T034`–`T037`)** → Default Combined `Screen 1 → Screen 2 → Screen 3` neon schedule and `00/08` VCR LED counter live.
6. **Polish (`T038`–`T040`)** → Full static type check, `/new` isolation check, `npm run lint`, `npm run build`, and `npm test` green.

---

## Notes

- `[P]` tasks operate on disjoint files and have no unmet dependencies within their phase; when running multiple phases concurrently across a team, coordinate edits to shared append-target files (`src/styles/tailwind.css` and `tests/export-rebrand.test.mjs`).
- Zero new `dependencies` or `devDependencies` may be added to `package.json` (YAGNI / Architectural Constraint #3).
- Keep `speakers.js` and `schedule.js` filenames and named exports (`Sessions`, `OVERALL_START`, `OVERALL_END`, `Schedule`) intact so `Schedule.jsx`, `usePicks.js`, and `/new` continue working without modification to `src/new/**`.

