# Quickstart & Validation Guide: 80s Miami Vice / New Wave Conference Rebrand

**Feature**: `001-miami-vice-rebrand`  
**Related Artifacts**:
- Data Model: `specs/001-miami-vice-rebrand/data-model.md`
- Data & Asset Contract: `specs/001-miami-vice-rebrand/contracts/speakers-schedule-contract.md`
- Theme & UI Contract: `specs/001-miami-vice-rebrand/contracts/theme-tokens-and-ui-contract.md`

---

## 1. Prerequisites & Local Setup

- **Node.js**: v18 LTS or v20 LTS (matches `.github/workflows/deploy.yml`)
- **Package Manager**: `npm` (`package-lock.json` is committed at repository root; zero new runtime or UI dependencies are installed)

```bash
npm ci
npm run dev
```

Open `http://localhost:3000` to inspect the primary live site (`/`) and `http://localhost:3000/new` to verify unpublished preview compatibility.

---

## 2. Build, Lint & Automated Verification Commands

Run the full validation pipeline before opening a pull request:

```bash
# 1. Lint Next.js / React source and accessibility rules
npm run lint

# 2. Build static export (out/) and generate sitemap.xml
npm run build

# 3. Run zero-dependency Node test suite (data invariants, WCAG AA contrast math, zero-hex check, exported HTML checks)
node --test tests/**/*.test.mjs
```

### Expected Automated Check Outcomes
1. **Schedule & Speaker Integrity (`speakers-schedule-contract.md`)**:
   - `Sessions` in `speakers.js` exports exactly `24` speakers (`1` Keynote: `DanBeglin`; `23` Talks including `LuciaCaminosJasonDuval` and `KenRosenberg`; `SimonStevens` and `MikolajMaciejak` removed).
   - All `24` `(screen, timeWindow)` slots across `Screen 1`, `Screen 2`, and `Screen 3` are populated with zero duplicates and zero empty slots.
   - `public/assets/lucia-jason.svg` and `public/assets/ken-rosenberg.svg` exist with valid `800×800` `viewBox` dimensions.
   - `/new` static export pages (`out/new.html`, `out/new/schedule.html`, `out/new/speakers/*.html`) build cleanly without errors.
2. **Design Token & Contrast Audit (`theme-tokens-and-ui-contract.md`)**:
   - Zero hardcoded `#hex` color literals appear inside `src/components/**` or `src/pages/**`.
   - Every semantic foreground/background and UI border pair in `src/styles/tokens.css` passes WCAG 2.1 Level AA (`≥ 4.5:1` text, `≥ 3.0:1` UI borders/large text) in both `miami-night` and `pastel-day`.
3. **Static Export Rebrand Audit (`out/`)**:
   - Canonical exported pages (`out/index.html`, `out/schedule.html`, `out/speakers.html`, `out/speakers/*.html`, `out/code-of-conduct.html`, `out/past-events.html`, `out/how-to-convince-your-boss.html`) contain `"Dan Beglins Mega Brum Tech Fest"`, zero current-edition `"TechMids Conf"` strings, and retain historical titles inside `out/past-events.html`.

---

## 3. End-to-End Interactive Validation Scenarios

### Scenario A — Zero-Flash Theme Switching & Persistence (US2)
1. Open `http://localhost:3000` in a fresh incognito window (even with OS set to Light Mode).
2. **Expected**: `<html data-theme="miami-night">` is active on first paint (midnight purple canvas, hot magenta borders, electric cyan highlights).
3. Press `Tab` until the **Miami Night / Pastel Day** switcher in the header receives focus; press `Space` or `Enter`.
4. **Expected**: Visible focus ring surrounds the button, palette switches immediately to **Pastel Day**, `aria-pressed` / label updates, and reloading or navigating to `/schedule` keeps **Pastel Day** active with zero flash of Miami Night.

### Scenario B — Synthwave Hero, Responsive Viewports & Reduced Motion (US1)
1. Inspect `/` at `320px`, `768px`, and `1440px` viewport widths.
2. **Expected**: Hero displays **"Dan Beglins Mega Brum Tech Fest"** and **"Better than tech mids"** above the horizontal-striped sunset and perspective grid floor with zero horizontal scrollbar (`document.documentElement.scrollWidth <= window.innerWidth`).
3. Emulate `prefers-reduced-motion: reduce` in browser DevTools.
4. **Expected**: Perspective grid floor and neon glows render crisply with zero animation.

### Scenario C — 80s Duotone Speaker Cards & Headliner Keynote (US3)
1. Browse `/#speakers` and `/speakers`.
2. **Expected**: **Dan Beglin** displays a glowing neon **`HEADLINER`** badge; **Lucia Caminos & Jason Duval** and **Ken Rosenberg** display custom 80s retro portraits; all 24 cards render in magenta/cyan duotone at rest.
3. Hover over cards with a pointer and `Tab` through speaker links via keyboard.
4. **Expected**: Each portrait snaps from magenta/cyan duotone to full natural color on hover and keyboard focus, and `/speakers/DanBeglin`, `/speakers/LuciaCaminosJasonDuval`, and `/speakers/KenRosenberg` display accurate session times, screens, and talk abstracts.

### Scenario D — 3-Screen Arcade Schedule & VCR LED Counter (US4)
1. Open `/schedule` (or scroll to `#schedule` on `/`).
2. **Expected**: Defaults to **Combined** view ordered **Screen 1 → Screen 2 → Screen 3**; Welcome slot reads *"Welcome to Dan Beglins Mega Brum Tech Fest!"*; Screen 1, Screen 2, and Screen 3 show three distinct neon border colors; `"My day"` control displays VCR LED counter `00/08`.
3. Click **Pick** on Dan Beglin (`09:45`, Screen 3), Lucia Caminos & Jason Duval (`11:15`, Screen 2), and Ken Rosenberg (`16:30`, Screen 1).
4. **Expected**: Counter updates immediately (`01/08` → `02/08` → `03/08`), competing talks in those rows recede visually, and switching to **My day** shows the chosen itinerary with working **Share my picks** (`Link copied` inline feedback) and **Clear picks** (`00/08`) controls.

