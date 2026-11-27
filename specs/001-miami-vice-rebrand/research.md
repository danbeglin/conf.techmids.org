# Technical Research: 80s Miami Vice / New Wave Conference Rebrand & Visual Overhaul

**Feature**: `001-miami-vice-rebrand`  
**Date**: 2026-11-27  
**Specification**: `specs/001-miami-vice-rebrand/spec.md`

## Overview

Phase 0 research resolves all architectural, styling, accessibility, compatibility, and testing decisions required to overhaul the primary conference site (`/`) into **Dan Beglins Mega Brum Tech Fest** (*"Better than tech mids"*) while obeying the TechMids Conf Constitution (v1.0.0) and the three mandatory architectural constraints:

1. **Data & Schedule Integrity (`speakers.js`)**
2. **Centralized Theme Tokens & Zero-Flash Bootstrapping**
3. **Pure CSS Retro Visuals & Reduced-Motion Guardrails**

Zero `NEEDS CLARIFICATION` items remain.

---

## R1. Single-Source Speaker & Slot Assignments in `speakers.js` Without Breaking `/new` or Schedule Parsers

- **Decision**: Keep `export const Sessions` in `speakers.js` with its exact existing object schema (`name`, `jobTitle`, `bio`, `talkTitle`, `description`, `image`, social URLs, `sessionType`, `speakerCard`, `speakerCardImage`) while adding an additive `slot` descriptor (`{ screen: 'screen1' | 'screen2' | 'screen3', start: string, end: string }`) to every speaker entry in `speakers.js`. Refactor `schedule.js` once into a thin, deterministic builder over `Sessions` (without importing `event.js`, preserving the unidirectional dependency graph `event.js → schedule.js → speakers.js`) that retains `OVERALL_START`, `OVERALL_END`, and `Schedule` (`conferenceSchedule`, `screen1`, `screen2`, `screen3`).
- **Rationale**:
  - Currently, speaker identities and talk titles are duplicated as 24 standalone `const` objects in `schedule.js`, and removing `SimonStevens` or `MikolajMaciejak` from `speakers.js` alone causes `Schedule.jsx` (`Sessions[speakerId].image`) and `/new` (`src/new/data.js`) to fail or show stale entries.
  - Storing speaker profiles, keynote flags (`sessionType: "Keynote"`), and session slot assignments (`slot: { screen, start, end }`) exclusively inside `speakers.js` establishes a single source of truth while preserving `export const Sessions` for all 7 existing consumer modules (`Speakers.jsx`, `Speaker.jsx`, `Schedule.jsx`, `EventStructuredData.jsx`, `usePicks.js`, `src/new/data.js`, `src/pages/speakers/[speaker].jsx`, `src/pages/new/speakers/[speaker].jsx`, `src/pages/sharing/[speaker].jsx`).
  - `event.js` imports `{ OVERALL_END, OVERALL_START } from "schedule"` on line 1; keeping `schedule.js` free of imports from `event.js` avoids a circular ES module initialization hazard while updating `welcome.description` to `'Welcome to Dan Beglins Mega Brum Tech Fest!'`.
  - Existing client-side itinerary reconciliation in `src/picks/usePicks.js` compares saved picks against `src/picks/baseline.js` and `Schedule`, automatically alerting returning visitors if they had previously picked `SimonStevens`, `MikolajMaciejak`, or `DanBeglin`'s old `16:30` slot.
- **Alternatives considered**:
  - *Hardcoding new speaker objects manually in both `speakers.js` and `schedule.js`*: Rejected because it violates Architectural Constraint #1 (modifying speaker profiles, keynote flags, and session slot assignments exclusively inside `speakers.js`) and invites drift between speaker talk titles and schedule cells.
  - *Changing `Sessions` from an object map to an array or renaming `sessionType: "Keynote"` to `"HEADLINER"` in raw data*: Rejected because `src/new/SpeakerCard.jsx` checks `speaker.sessionType === 'Keynote'` and `getStaticPaths` / `Schedule.jsx` index `Sessions[speakerId]` by key; keeping `sessionType: "Keynote"` in data and mapping `"Keynote" → "HEADLINER"` in the live site's presentation badge preserves 100% `/new` compatibility.

---

## R2. Static 80s Retro SVG Portrait Assets & Next.js Unoptimized Image Compatibility

- **Decision**: Author two full-color, multi-luminance 80s retro vector portraits at `public/assets/lucia-jason.svg` and `public/assets/ken-rosenberg.svg` with explicit `width="800" height="800" viewBox="0 0 800 800"` attributes, and import them into `speakers.js` via standard ES module static imports (`import LuciaCaminosJasonDuval from "./public/assets/lucia-jason.svg"` and `import KenRosenberg from "./public/assets/ken-rosenberg.svg"`).
- **Rationale**:
  - `next.config.js` configures `output: "export"` and `images: { unoptimized: true }` with Next.js `13.3.1` and no custom `@svgr/webpack` override. Next.js's built-in `next-image-loader` resolves static `.svg` imports into `{ src, width, height }` metadata objects identical in shape to `.jpeg` imports.
  - This satisfies **both** consumption patterns across the repository:
    1. Primary site (`Speakers.jsx`, `Speaker.jsx`, `Schedule.jsx`): `<Image src={speaker.image} ... />` requires either `fill` or numeric `width`/`height` extracted from the static import object (`StackedScheduleView` renders `<Image src={Sessions[speakerId].image} />` without `fill`, which throws a build-time error if `image` is a raw URL string).
    2. Unpublished preview (`src/new/SpeakerCard.jsx`, `src/new/ScheduleBoard.jsx`, `src/pages/new/speakers/[speaker].jsx`): `<img src={speaker.image.src} />` reads `.src` directly off the imported object.
  - Authoring the SVGs in rich full color (warm skin highlights, neon/pastel 80s attire, tropical blinds/sunset background gradients) gives `grayscale(100%) contrast(...)` + CSS blend modes distinct tonal separation for the resting magenta/cyan duotone effect while snapping to vibrant full-color artwork on `:hover` / `:focus-visible`.
- **Alternatives considered**:
  - *Referencing string paths (`image: "/assets/ken-rosenberg.svg"`) in `speakers.js`*: Rejected because `speaker.image.src` in `/new` evaluates to `undefined` and Next.js `<Image>` without `fill` in `Schedule.jsx` fails `next build`.
  - *Pre-baking magenta/cyan colors into the SVG artwork*: Rejected because hover/focus states must reveal natural full-color 80s illustrations rather than staying monochrome duotone.

---

## R3. Centralized CSS Custom Property Token Architecture & `/new` Isolation

- **Decision**: Create `src/styles/tokens.css` (imported at the top of `src/styles/tailwind.css`) defining all color, surface, border, neon glow, duotone, sunset, grid, VCR LED, and Screen 1 / Screen 2 / Screen 3 accent tokens under `:root, html[data-theme="miami-night"]` (default) and `html[data-theme="pastel-day"]`. Map Tailwind semantic utilities (`surface`, `ink`, `neon`, `screen1`, `screen2`, `screen3`, `vcr`, plus remapped `brand`/`techmids` aliases where shared) in `tailwind.config.js` to reference those CSS custom properties, banning hardcoded `#hex` values in `src/components/**` and `src/pages/**`.
- **Rationale**:
  - Directly satisfies Constitution Principle II (*Design Tokens Only*) and Architectural Constraint #2.
  - Scoping root theme attributes on `html[data-theme="..."]` updates the entire document instantaneously (`<16ms`, single style recalculation, zero React re-render cascade required for styling) when switching between **Miami Night** and **Pastel Day**.
  - `src/new/tokens.css` and `src/new/site.css` scope all `/new` preview rules to `.tm-site` with explicit `background: var(--tm-bg); color: var(--tm-text);`, ensuring `html[data-theme]` on the document root does not visually alter `/new`.
- **Alternatives considered**:
  - *Tailwind `dark:` class variant alone*: Rejected because Constraint #2 explicitly requires `data-theme="miami-night"` (default) and `data-theme="pastel-day"` attributes, and semantic multi-theme CSS variables avoid duplicating `dark:...` utility chains on every element.
  - *React Context inline styles*: Rejected because runtime JS style injection causes hydration flashes and violates pure CSS token centralization.

---

## R4. Zero-Flash Blocking Theme Bootstrapper in `_document.jsx`

- **Decision**: Set `data-theme="miami-night"` directly on `<Html lang="en" data-theme="miami-night">` in `src/pages/_document.jsx` for static HTML output, and inject a synchronous, `try/catch`-guarded inline `<script>` as the first child of `<Head>` that reads `localStorage.getItem('theme')` and sets `document.documentElement.setAttribute('data-theme', value === 'pastel-day' ? 'pastel-day' : 'miami-night')` before first paint.
- **Rationale**:
  - Static export (`next build` with `output: "export"`) serves pre-rendered HTML with no per-request server rendering. Defaulting `<Html data-theme="miami-night">` guarantees first-time visitors and no-JS environments immediately paint **Miami Night** regardless of OS `prefers-color-scheme` (FR-007, US2 Scenario 1).
  - Executing the tiny synchronous inline script before CSS paint restores `'pastel-day'` when saved, ignores legacy `'dark'`/`'light'` values from prior site versions, and swallows `SecurityError` exceptions in strict private browsing / sandboxed iframes without crashing (Edge Case requirement).
- **Alternatives considered**:
  - *Reading `localStorage` inside `useEffect` in `_app.jsx` or `Header.jsx`*: Rejected because `useEffect` runs after hydration/first paint, causing a visible Miami Night → Pastel Day flash on reload (violating SC-002 and Constraint #2).
  - *Falling back to `window.matchMedia('(prefers-color-scheme: light)')`*: Rejected because FR-007 explicitly requires **Miami Night** as the unconditional default for first-time visitors regardless of OS dark/light setting.

---

## R5. Pure CSS Synthwave Visuals & `prefers-reduced-motion` Guardrails

- **Decision**: Implement all four signature 80s visual motifs in native CSS (`src/styles/synthwave.css` / `@layer components` in `src/styles/tailwind.css`) with zero external UI or animation libraries:
  1. **Striped Synthwave Sunset**: Circular `linear-gradient` disc segmented by horizontal cutout bands via `mask-image: linear-gradient(...)` / `repeating-linear-gradient(...)` (with `-webkit-mask-image` prefix for Safari/iOS WebKit) and multi-layer `box-shadow` horizon bloom.
  2. **Perspective Grid Floor**: Clipped horizon stage (`overflow: hidden`) containing a `transform: perspective(28rem) rotateX(68deg)` plane with orthogonal `linear-gradient` grid lines (`background-size: 3rem 3rem`) and horizon fade mask.
  3. **Magenta/Cyan Duotone Headshots**: Container isolated with `isolation: isolate` and cyan highlight base (`var(--mv-duotone-cyan)`), applying `filter: grayscale(100%) contrast(1.15)` + `mix-blend-mode: multiply` on the portrait image and a `mix-blend-mode: screen` magenta shadow layer (`var(--mv-duotone-magenta)`) via `::after`. Snaps cleanly to `filter: none; mix-blend-mode: normal` and `::after { opacity: 0 }` on card `:hover` and keyboard `:focus-visible` / `:focus-within`.
  4. **VCR LED Counter & 3-Screen Neon Borders**: Tabular-numeral (`font-variant-numeric: tabular-nums`) segmented readout (`00/08`) inside a dark recessed bevel (`inset` `box-shadow` + subtle scanline `repeating-linear-gradient` + `text-shadow` phosphor glow), paired with dedicated token sets for **Screen 1** (Hot Magenta), **Screen 2** (Electric Cyan), and **Screen 3** (Solar Amber / Arcade Violet) and defaulting `Schedule.jsx` to the **Combined** view ordered `Screen 1 → Screen 2 → Screen 3`.
  5. **Reduced Motion**: Wrap any grid-scroll keyframe animation and glow/filter transitions in `@media (prefers-reduced-motion: no-preference)` AND explicitly reset `animation: none !important; transition-duration: 0.01ms !important;` inside `@media (prefers-reduced-motion: reduce)`.
- **Rationale**:
  - Uses GPU-composited native CSS primitives already supported across modern Chromium, Firefox, and Safari/iOS browsers (`browserslist: "defaults, not ie <= 11"`), adding `0 KB` of JavaScript bundle weight.
  - Applying CSS blend modes on the wrapper around `<Image>` works uniformly for `.jpeg` photos and `.svg` illustrations without Canvas pixel manipulation or CORS/tainting issues.
- **Alternatives considered**:
  - *SVG `<feColorMatrix>` filter IDs referenced via `filter: url(#duotone)`*: Considered as viable, but CSS `grayscale() + mix-blend-mode` avoids cross-Safari clipPath+SVG-filter compositing quirks (especially since `Speakers.jsx` previously used inline SVG `clipPath` IDs) and responds directly to CSS custom properties when switching themes.
  - *Framer Motion / Three.js / GSAP for perspective grid*: Explicitly forbidden by Constraint #3 and Constitution Principle I (YAGNI).

---

## R6. Verified WCAG 2.1 Level AA Contrast Tokens Across Both Themes

- **Decision**: Lock token hex values in `src/styles/tokens.css` only after verifying relative luminance contrast ratios against WCAG 2.1 Level AA thresholds (≥ `4.5:1` normal text, ≥ `3.0:1` large text and non-text UI borders/indicators):
  - **Miami Night (`data-theme="miami-night"`)**:
    - Canvas `#120824`, Surface `#1B0E36`, Elevated `#26144A`
    - Primary ink `#F8F5FF` (`17.95:1` on canvas, `16.77:1` on surface, `15.22:1` on elevated)
    - Muted ink `#D4C6F0` (`12.10:1` on canvas, `11.30:1` on surface, `10.25:1` on elevated)
    - Screen 1 (Hot Magenta): border `#FF2A85` (`5.09:1` on surface), text `#FF6BB5` (`6.90:1` on surface)
    - Screen 2 (Electric Cyan): border & text `#00F0FF` (`12.82:1` on surface)
    - Screen 3 (Solar Amber): border & text `#FFB800` (`10.41:1` on surface)
    - `HEADLINER` badge: `#120824` ink on `#00F0FF` or `#FFFFFF` ink on `#C80064` (`13.73:1` / `5.77:1`)
    - VCR LED counter: `#39FF88` / `#00F0FF` digits on `#0D051A` recessed bezel (`>13.7:1`)
  - **Pastel Day (`data-theme="pastel-day"`)**:
    - Canvas `#FFF6FA`, Surface `#FFFFFF`, Elevated `#F4EAFF`
    - Primary ink `#1B0E33` (`17.12:1` on canvas, `18.15:1` on surface, `15.61:1` on elevated)
    - Muted ink `#4E386E` (`9.37:1` on canvas, `9.93:1` on surface, `8.54:1` on elevated)
    - Screen 1 (Flamingo Magenta): border `#D40072` (`5.19:1` on surface), text `#A80058` (`7.48:1` on surface, `6.44:1` on elevated)
    - Screen 2 (Biscayne Cyan): border `#00889E` (`4.19:1` on surface), text `#006678` (`6.62:1` on surface, `5.69:1` on elevated)
    - Screen 3 (Ocean Drive Violet): border `#7C3AED` (`5.70:1` on surface), text `#5B21B6` (`8.98:1` on surface, `7.75:1` on elevated)
    - `HEADLINER` badge: `#FFFFFF` ink on `#A80058` fill (`7.48:1`)
    - VCR LED counter: Dark recessed display window `#1A0D2E` with `#00F0FF` digits (`13.07:1`) — preserves authentic physical VCR LED window appearance and daylight readability.
- **Rationale**: Raw neon cyan (`#00F0FF`) or neon yellow (`#FFFF00`) directly on white/pastel surfaces yields `<1.5:1` contrast and fails WCAG AA catastrophically. Splitting semantic tokens into Night neon emissives vs. Day saturated Miami Art Deco inks/borders—while keeping the VCR LED readout inside a dark recessed bezel—preserves the unmistakable 1980s Miami aesthetic with 100% WCAG AA (and mostly AAA) compliance.
- **Alternatives considered**:
  - *Using identical neon foreground colors on light pastel cards with text-shadow outlines*: Rejected because WCAG 2.1 contrast calculations do not reliably credit blurred glow shadows and fail automated axe/Lighthouse contrast audits.

---

## R7. Site-Wide Rebranding Touchpoints & Archive Boundary

- **Decision**: Centralize event identity strings in `event.js` (`title: "Dan Beglins Mega Brum Tech Fest"`, `tagline: "Better than tech mids"`, `organizerName: "Dan Beglins Mega Brum Tech Fest"`, `edition` getter returning `"Dan Beglins Mega Brum Tech Fest"`) and replace all hardcoded `"TechMids Conf"` / `"Tech Mids"` / legacy SVG wordmarks across:
  - Graphical logos: `src/components/Logo.jsx` (header/footer synthwave wordmark SVG using CSS token variables, zero hardcoded hex) and `src/components/LogoConf.jsx` (hero synthwave emblem/lockup SVG using CSS token variables). Note: `src/new/ui.jsx` defines its own separate `Wordmark` and `BlockLogo` components, so updating `Logo.jsx` and `LogoConf.jsx` affects only the primary live site.
  - Layout & metadata: `Header.jsx`, `Hero.jsx`, `Footer.jsx`, `PageMeta.jsx` (`og:site_name`), `EventStructuredData.jsx`, `Schedule.jsx` / `schedule.js` welcome banner, `SharingCard.jsx`, and canonical page wrappers (`src/pages/index.jsx`, `src/pages/schedule.jsx`, `src/pages/speakers/index.jsx`, `src/pages/speakers/[speaker].jsx`, `src/pages/code-of-conduct.jsx`, `src/pages/past-events.jsx`, `src/pages/how-to-convince-your-boss.jsx`, `src/components/CodeOfConduct.jsx`).
  - Historical archive preservation: Keep historical archive titles inside `src/components/PastEventsList.jsx` (`'The inaugural TechMids Conf'`, `'TechMids Conf October 2023'`, etc.) and historical dated attendee attributions in `Testimonials.jsx` intact per FR-001 clarification.
- **Rationale**: Eliminates scattered hardcoded conference name strings so every live route, `<title>`, OpenGraph tag, JSON-LD `Event` schema, header, hero, welcome slot, and footer copyright reflects **Dan Beglins Mega Brum Tech Fest** consistently.
- **Alternatives considered**:
  - *Find-and-replace across the entire repo blindly*: Rejected because it would corrupt `/new` preview pages (`src/pages/new/**`), `PastEventsList.jsx` historical archive records, `localStorage` pick keys (`techmids:picks:2026-11-27` shared with `usePicks.js`), and external ticketing URLs (`ticketLink` / `titoId`).

---

## R8. Strict Static Typing & Zero-Dependency User-Centric Testing Strategy

- **Decision**:
  1. **Strict Typing**: Author `src/types/conference.d.ts` with strict TypeScript definitions (`ThemeId`, `ScreenId`, `SessionType`, `ScheduleSlotAssignment`, `SpeakerEntry`, `SessionsMap`, `ScheduleSlot`, `TrackSchedule`, `ScheduleMap`, and component prop contracts) and annotate all new/modified `.js`/`.jsx` modules (`speakers.js`, `schedule.js`, `event.js`, theme switcher, hero, speaker cards/detail, schedule/VCR counter) with `@ts-check` + JSDoc `@type` / `@param` contracts (zero `any`).
  2. **Automated Verification (`tests/`)**: Use Node.js's built-in `node:test` + `node:assert/strict` runner (`npm test` -> `node --test tests/**/*.test.mjs`) requiring **zero external packages** to verify:
     - Schedule & speaker invariants (exact 24 speakers, Dan Beglin Keynote at `Screen 3 09:45`, Lucia & Jason + Ken Rosenberg placed with zero duplicates or empty slots, `/new` `buildRows()` compatibility, SVG portrait asset dimensions).
     - Token contrast & zero-hex governance (programmatic WCAG 2.1 AA contrast verification across every foreground/background/border pair in `src/styles/tokens.css`, plus static scan ensuring zero `#hex` literals in `src/components/` and `src/pages/`).
     - User-visible static export DOM checks against `out/` produced by `npm run build` (rebranded titles/headings/footer/welcome banner on every canonical route, inline `<head>` blocking `data-theme` script, `HEADLINER` badge on Keynote only, Combined `Screen 1 → Screen 2 → Screen 3` order, VCR LED counter readout, reduced-motion CSS block).
- **Rationale**: Satisfies Constitution Principle IV (*Strict Typing* during incremental JS migration + *User-Centric Testing* verifying rendered user-visible output) and Principle I (*Simplicity First / YAGNI*) while preserving the existing `.js`/`.jsx` Next.js 13 build pipeline in `.github/workflows/deploy.yml`.
- **Alternatives considered**:
  - *Migrating `speakers.js` to `speakers.ts` and installing TypeScript/Jest/Testing-Library*: Rejected because renaming `speakers.js` risks breaking explicit imports and violates Constraint #1 (*preserving `speakers.js`* untouched in export/module identity) and Constraint #3 / YAGNI (avoiding unnecessary dependency churn).

