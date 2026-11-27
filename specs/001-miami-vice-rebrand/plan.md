# Implementation Plan: 80s Miami Vice / New Wave Conference Rebrand & Visual Overhaul

**Branch**: `001-miami-vice-rebrand` | **Date**: 2026-11-27 | **Spec**: `specs/001-miami-vice-rebrand/spec.md`

**Input**: Feature specification from `specs/001-miami-vice-rebrand/spec.md`

## Summary

Rebrand the primary public conference website (`/`) into **Dan Beglins Mega Brum Tech Fest** with the hero tagline **"Better than tech mids"** and overhaul its visual experience into a 1980s Miami Vice / New Wave aesthetic while preserving full backward compatibility with the 3-screen schedule parser and unpublished `/new` preview. Technically, the implementation:

1. **Centralizes speaker & slot data in `speakers.js`** (promoting Dan Beglin to Keynote/`HEADLINER` at `Screen 3, 09:45–10:30`, adding co-speakers Lucia Caminos & Jason Duval at `Screen 2, 11:15–12:00` and Ken Rosenberg at `Screen 1, 16:30–17:15` with static `800×800` full-color 80s retro SVG portraits in `public/assets/`, removing Simon Stevens and Mikolaj Maciejak, and deriving `schedule.js` tracks directly from `speakers.js`).
2. **Establishes a single root CSS custom-property token layer (`src/styles/tokens.css`)** driven by `html[data-theme="miami-night"]` (default) and `html[data-theme="pastel-day"]`, backed by a synchronous blocking `<head>` script in `src/pages/_document.jsx` for zero-flash theme restoration and zero hardcoded `#hex` values in components.
3. **Renders all retro visuals in pure CSS**—horizontal-striped sunset, 3D perspective grid floor, CSS blend-mode magenta/cyan duotone headshots snapping to full color on hover/focus, neon `HEADLINER` badge, distinct Screen 1 / Screen 2 / Screen 3 neon borders (defaulting to Combined `Screen 1 → Screen 2 → Screen 3`), and a tabular VCR LED `"My day"` counter—guarded by `@media (prefers-reduced-motion: reduce)` and verified to pass WCAG 2.1 Level AA contrast across both themes.

## Technical Context

**Language/Version**: JavaScript (ES2022 / JSX) with strict TypeScript declaration contracts (`src/types/conference.d.ts` + `@ts-check` JSDoc annotations) on Node.js 18/20 LTS

**Primary Dependencies**: Next.js `13.3.1` (Pages Router, `output: "export"`, `images: { unoptimized: true }`), React `18.2.0`, Tailwind CSS `3.1.3` (`@tailwindcss/typography`, `@tailwindcss/aspect-ratio`), `@headlessui/react` `1.6.5`, `@heroicons/react` `1.0.6`, `clsx` `1.1.1`, `moment` `2.29.4` — **zero new runtime, UI, or animation dependencies**

**Storage**: Static ES data modules (`speakers.js`, `schedule.js`, `event.js`); client-side `window.localStorage` (`theme` for Miami Night / Pastel Day preference; `techmids:picks:2026-11-27` + URL `#p=` hash for `"My day"` picks) with graceful in-memory fallback when storage is blocked

**Testing**: Built-in Node.js test runner (`node --test tests/**/*.test.mjs`), `next lint`, and `next build` static export verification (`out/`)

**Target Platform**: Static HTML/CSS/JS web export deployed to GitHub Pages; modern desktop and mobile browsers (`defaults, not ie <= 11`) from `320px` to `1440px+`

**Project Type**: Frontend static web application (Next.js Pages Router)

**Performance Goals**: Zero wrong-theme flash on initial load (`0ms` FOUC via blocking `<head>` script); `<100ms` full-viewport theme toggle via root `data-theme` attribute swap; `0` Cumulative Layout Shift (CLS) on speaker portraits and hero artwork; `0 KB` added third-party JS bundle weight

**Constraints**:
- **Constraint 1 (`speakers.js` Integrity)**: Author speaker profiles, keynote flags, and session slot assignments exclusively in `speakers.js`, keeping `export const Sessions` and its per-speaker schema intact for `Schedule.jsx`, `usePicks.js`, and `/new`; generate static full-color 80s retro SVG portraits in `public/assets/`.
- **Constraint 2 (Token Centralization & Zero-Flash)**: Define all palette, surface, border, glow, and Screen 1/2/3 accent variables in a single root CSS layer (`data-theme="miami-night"` default, `data-theme="pastel-day"`); ban hardcoded `#hex` values in component/page files; bootstrap `data-theme` synchronously in `<head>`.
- **Constraint 3 (Pure CSS & Accessibility Guardrails)**: Implement synthwave sunset, perspective grid floor, magenta/cyan duotone filter, and VCR LED counter purely in CSS with `@media (prefers-reduced-motion: reduce)` overrides and WCAG 2.1 Level AA contrast (`≥ 4.5:1` normal text, `≥ 3.0:1` UI components/large text) in both themes; keep `/new` visually untouched.

**Scale/Scope**: `24` speakers across `8` session windows × `3` cinema screens (`24` talk slots, `0` gaps), `2` themes (`miami-night`, `pastel-day`), `7` primary canonical page routes + `24` static speaker detail pages + `2` custom SVG portrait assets

## Constitution Check

*GATE: Passed prior to Phase 0 research and re-verified after Phase 1 design (`research.md`, `data-model.md`, `contracts/`, `quickstart.md`).*

- [x] **I. UI Architecture & Component Design**: Presentation components (`ThemeSwitcher.jsx`, `SynthwaveBackdrop.jsx`, `SpeakerPortrait.jsx`, `HeadlinerBadge.jsx`, `VcrCounter.jsx`, `Logo.jsx`, `LogoConf.jsx`) are small, single-responsibility, pure UI primitives decoupled from `speakers.js` / `usePicks.js` data wiring. Native CSS gradients, masks, blend modes, and `localStorage` APIs are used with **zero** new dependencies (YAGNI). Layouts use mobile-first Flexbox/Grid and fluid `rem`/`clamp()`/`%` units verified across `320px–1440px+` without horizontal overflow.
- [x] **II. User Experience & Visual Consistency**: 100% of colors, surfaces, borders, glows, and Screen 1/2/3 track accents live in `src/styles/tokens.css` and are mapped into `tailwind.config.js`; existing hardcoded hex literals in `Logo.jsx`, `LogoConf.jsx`, `Hero.jsx`, `Testimonials.jsx`, and `SharingCard.jsx` are replaced by tokens. Loading (pre-paint theme + fixed aspect-ratio frames), empty (`00/08` `"My day"` prompt + empty lineup fallback), error (broken-portrait monogram fallback, clipboard URL fallback, `try/catch` storage guard), and success (`aria-pressed` theme toggle, instant VCR LED counter increment, `"Link copied"` status) states are explicitly engineered.
- [x] **III. Accessibility (a11y) (NON-NEGOTIABLE)**: Semantic `<header>`, `<nav>`, `<main>`, `<section>`, `<table>`/`<ol>`, and `<button type="button">` elements are enforced across all views. Keyboard navigation (`Tab`, `Shift+Tab`, `Enter`, `Space`) triggers visible `--mv-focus-ring` indicators and snaps speaker cards from duotone to full color on `:focus-visible`. Every foreground/background and border token pair in `contracts/theme-tokens-and-ui-contract.md` is mathematically verified to exceed WCAG 2.1 AA (`≥ 4.5:1` text, `≥ 3.0:1` UI borders) in both `miami-night` and `pastel-day`, and `@media (prefers-reduced-motion: reduce)` disables perspective grid animation and glow transitions.
- [x] **IV. Code Quality & Testing**: Strict static type contracts are defined in `src/types/conference.d.ts` and enforced via `@ts-check` + JSDoc `@type`/`@param` annotations across modified `.js`/`.jsx` files with zero implicit `any` escapes while keeping `speakers.js` filename/export intact. Automated user-centric tests (`node:test`) validate schedule invariants, WCAG AA contrast ratios, zero hardcoded component hexes, and rendered static HTML output across `out/`.

## Project Structure

### Documentation (this feature)

```text
specs/001-miami-vice-rebrand/
├── plan.md                                    # Technical implementation plan (this file)
├── research.md                                # Phase 0 architectural & contrast research
├── data-model.md                              # Phase 1 entities, 24-slot matrix, state transitions
├── quickstart.md                              # Phase 1 build, audit, and interactive validation guide
├── contracts/
│   ├── speakers-schedule-contract.md          # speakers.js, schedule.js, /new & SVG asset contracts
│   └── theme-tokens-and-ui-contract.md        # CSS token palette, WCAG AA matrix & UI/ARIA contracts
└── tasks.md                                   # Phase 2 task list (generated next via /speckit-tasks)
```

### Source Code (repository root)

```text
event.js                                       # Rebranded EVENT identity, tagline & edition metadata
speakers.js                                    # Single source of truth: 24 speakers, Keynote flag & slot assignments
schedule.js                                    # Deterministic 3-screen schedule builder derived from speakers.js
tailwind.config.js                             # Semantic Tailwind color/glow/font mappings referencing tokens.css

public/
├── assets/
│   ├── lucia-jason.svg                        # Static 800×800 full-color 80s retro co-speaker portrait
│   └── ken-rosenberg.svg                      # Static 800×800 full-color 80s retro speaker portrait
└── speakers/                                  # Existing speaker photographs (including DanBeglin.jpeg)

src/
├── types/
│   └── conference.d.ts                        # Strict static TypeScript contracts for data models & UI props
├── styles/
│   ├── tokens.css                             # Root CSS custom properties for miami-night & pastel-day
│   └── tailwind.css                           # Imports tokens.css + pure CSS synthwave/duotone/VCR utilities
├── components/
│   ├── ThemeSwitcher.jsx                      # Accessible Miami Night / Pastel Day header switcher
│   ├── SynthwaveBackdrop.jsx                  # Pure CSS striped sunset & 3D perspective grid floor
│   ├── SpeakerPortrait.jsx                    # Duotone-to-color portrait frame with monogram error fallback
│   ├── HeadlinerBadge.jsx                     # Neon HEADLINER badge for Keynote sessions
│   ├── VcrCounter.jsx                         # Recessed tabular VCR LED counter (00/08)
│   ├── Logo.jsx                               # Token-driven synthwave header/footer wordmark SVG
│   ├── LogoConf.jsx                           # Token-driven synthwave hero lockup SVG
│   ├── Header.jsx                             # Rebranded header integrating ThemeSwitcher
│   ├── Hero.jsx                               # Rebranded hero ("Better than tech mids" + SynthwaveBackdrop)
│   ├── Footer.jsx                             # Rebranded organizer attribution & copyright
│   ├── Speakers.jsx                           # 80s speaker grid with SpeakerPortrait & HeadlinerBadge
│   ├── Speaker.jsx                            # Rebranded speaker detail view with HeadlinerBadge & slot info
│   ├── Schedule.jsx                           # Default Combined S1→S2→S3 neon schedule & VcrCounter
│   ├── PageMeta.jsx                           # Rebranded og:site_name & canonical metadata
│   ├── EventStructuredData.jsx                # Rebranded Schema.org Event JSON-LD
│   ├── CodeOfConduct.jsx                      # Rebranded current-event title references & token surfaces
│   ├── PastEventsList.jsx                     # Token surfaces while preserving historical archive names
│   └── Testimonials.jsx                       # Token surfaces/gradients while preserving historical quotes
├── pages/
│   ├── _document.jsx                          # Default data-theme="miami-night" + blocking <head> script
│   ├── index.jsx                              # Rebranded homepage metadata & layout
│   ├── schedule.jsx                           # Rebranded schedule route metadata & empty/ready states
│   ├── speakers/
│   │   ├── index.jsx                          # Rebranded speaker directory route
│   │   └── [speaker].jsx                      # Rebranded dynamic speaker profile route
│   ├── code-of-conduct.jsx                    # Rebranded Code of Conduct route metadata
│   ├── past-events.jsx                        # Rebranded Past Events route metadata
│   └── how-to-convince-your-boss.jsx          # Rebranded business-case copy & token surfaces
├── picks/
│   ├── usePicks.js                            # Existing client-side My day persistence & reconciliation
│   └── baseline.js                            # Existing schedule-change baseline (preserved)
└── new/                                       # Unpublished /new preview (visually untouched; shared data verified)

tests/
├── data-schedule.test.mjs                     # Verifies 24-slot bijection, Keynote rules, SVG assets & /new data
├── tokens-contrast.test.mjs                   # Verifies WCAG 2.1 AA contrast ratios & zero hardcoded hexes
└── export-rebrand.test.mjs                    # Verifies built out/ HTML branding, bootstrap script & UI markers
```

**Structure Decision**: Retain the repository's existing Next.js 13 Pages Router root-data + `src/` architecture (`event.js`, `speakers.js`, `schedule.js`, `src/components/`, `src/pages/`, `src/styles/`), adding `src/styles/tokens.css` as the single design-token layer, `src/types/conference.d.ts` for strict static contracts, focused pure subcomponents under `src/components/`, static portraits under `public/assets/`, and zero-dependency `node:test` suites under `tests/`.

## Architectural Implementation Blueprint

### 1. Data & Schedule Integrity (`speakers.js` → `schedule.js`)
- Author `public/assets/lucia-jason.svg` and `public/assets/ken-rosenberg.svg` as self-contained `800×800` full-color vector illustrations with broad shadow-to-highlight luminance separation.
- In `speakers.js`:
  - Remove `SimonStevens` and `MikolajMaciejak` imports and entries.
  - Update `DanBeglin` (`sessionType: "Keynote"`, `jobTitle: "Founder & Chief Synthwave Architect"`, `talkTitle: "Why Mega Brum Tech Fest Beats Everything Else"`, `slot: { screen: "screen3", start: "09:45 AM", end: "10:30 AM" }`).
  - Add `LuciaCaminosJasonDuval` (`name: "Lucia Caminos & Jason Duval"`, `jobTitle: "Staff Co-Founders @ Leonida Systems"`, `talkTitle: "Pair Programming Under Pressure: Shipping Fast When the Heat Is On"`, `sessionType: "Talk"`, `image: LuciaCaminosJasonDuval`, `slot: { screen: "screen2", start: "11:15 AM", end: "12:00 PM" }`).
  - Add `KenRosenberg` (`name: "Ken Rosenberg"`, `jobTitle: "Principal Incident & Compliance Counsel @ Rosenberg & Associates"`, `talkTitle: "Don't Panic! Survive P1 Production Outages Without Losing Your License"`, `sessionType: "Talk"`, `image: KenRosenberg`, `slot: { screen: "screen1", start: "16:30 PM", end: "17:15 PM" }`).
  - Add `slot: { screen, start, end }` to all 21 retained community speakers.
- Refactor `schedule.js` to construct `screen1Schedule`, `screen2Schedule`, and `screen3Schedule` dynamically from `Sessions` in `speakers.js`, update `welcome.description` to `'Welcome to Dan Beglins Mega Brum Tech Fest!'`, and export `Schedule` ordered `conferenceSchedule → screen1 → screen2 → screen3` without importing `event.js`.

### 2. Centralized Theme Tokens & Zero-Flash Bootstrapping
- Define `:root, html[data-theme="miami-night"]` and `html[data-theme="pastel-day"]` in `src/styles/tokens.css` per `contracts/theme-tokens-and-ui-contract.md`, covering canvas/surface/elevated backgrounds, primary/muted inks, hot magenta & electric cyan accents, Screen 1 / Screen 2 / Screen 3 border/text/glow tokens, `HEADLINER` badge tokens, sunset/grid/duotone stops, and VCR LED bezel/digit tokens.
- Map semantic token utilities into `tailwind.config.js` and purge all hardcoded `#hex` values from `src/components/**` and `src/pages/**` (including `Logo.jsx`, `LogoConf.jsx`, `Hero.jsx`, `Testimonials.jsx`, and `SharingCard.jsx`).
- Update `src/pages/_document.jsx` to render `<Html lang="en" data-theme="miami-night" className="antialiased">` and execute the synchronous `localStorage` `data-theme` bootstrapper script at the top of `<Head>`.
- Replace `ThemeToggle` in `Header.jsx` with `ThemeSwitcher.jsx` supporting pointer and keyboard toggling (`aria-pressed`, descriptive `aria-label`, visible `--mv-focus-ring`, instant `data-theme` swap, `try/catch` persistence).

### 3. Event Rebranding & Pure CSS Retro UI Components
- **Branding**: Update `event.js` (`title: "Dan Beglins Mega Brum Tech Fest"`, `tagline: "Better than tech mids"`), replace `Logo.jsx` and `LogoConf.jsx` with token-driven synthwave SVG wordmarks, and update current-edition branding in `Header.jsx`, `Hero.jsx`, `Footer.jsx`, `PageMeta.jsx`, `EventStructuredData.jsx`, `CodeOfConduct.jsx`, and canonical route pages under `src/pages/` while preserving historical titles in `PastEventsList.jsx` and leaving `src/new/**` / `src/pages/new/**` untouched.
- **Synthwave Hero**: Compose `Hero.jsx` with `SynthwaveBackdrop.jsx` (`.mv-sunset` horizontal-striped gradient mask + `.mv-grid-floor` 3D `perspective()`/`rotateX()` CSS grid inside an `overflow-hidden` container).
- **80s Speaker Cards & Detail**: Render `SpeakerPortrait.jsx` with CSS `grayscale(100%) contrast(1.15)` + `mix-blend-mode: multiply` / `screen` duotone layers snapping to full color on `:hover` and `:focus-visible`/`:focus-within`, retro monogram `onError` fallback, and `HeadlinerBadge.jsx` (`HEADLINER`) when `sessionType === "Keynote"`.
- **VHS / Arcade 3-Screen Schedule**: Default `Schedule.jsx` to `'combined'` view (unless `#p=` / `#picks=` hash opens `'myday'`), order `ROOMS` as `Screen 1 → Screen 2 → Screen 3`, apply `--mv-screen-{1,2,3}-*` neon borders/indicators consistently across Combined, Stacked, and My day views, and render `VcrCounter.jsx` (`00/08` tabular glowing readout) inside the `"My day"` control.
- **Reduced Motion**: Gate perspective grid motion and glow transitions behind `@media (prefers-reduced-motion: no-preference)` and explicitly neutralize animations/transitions inside `@media (prefers-reduced-motion: reduce)`.

### 4. Strict Typing & Verification Suite
- Add `src/types/conference.d.ts` and `@ts-check` JSDoc annotations across modified modules.
- Add `tests/data-schedule.test.mjs`, `tests/tokens-contrast.test.mjs`, and `tests/export-rebrand.test.mjs` using `node:test` to enforce 24-slot schedule integrity, `/new` compatibility, WCAG 2.1 AA contrast math, zero hardcoded component hexes, and static export HTML assertions.

## Complexity Tracking

*No Constitution violations or new third-party dependencies introduced.*

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| None | N/A | N/A |

