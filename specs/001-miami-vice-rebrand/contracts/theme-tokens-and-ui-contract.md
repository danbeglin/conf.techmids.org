# Contract: Centralized Theme Tokens, Zero-Flash Bootstrap & Retro UI Components

**Feature**: `001-miami-vice-rebrand`  
**Governs**: Architectural Constraints #2 & #3, Constitution Principles I–III (Design Tokens Only, Explicit UI States, WCAG 2.1 AA Accessibility)

---

## 1. Root Theme Attribute & Pre-Paint Bootstrapper Contract

### HTML Root Attribute
- Document root `<html>` MUST carry `data-theme="miami-night"` (default) or `data-theme="pastel-day"`.
- Static SSR markup in `src/pages/_document.jsx` MUST output `<Html lang="en" data-theme="miami-night" className="antialiased">`.

### Blocking `<Head>` Script (`src/pages/_document.jsx`)
Executed synchronously before stylesheets paint:

```js
(function () {
  try {
    var saved = window.localStorage.getItem('theme');
    var theme = saved === 'pastel-day' ? 'pastel-day' : 'miami-night';
    document.documentElement.setAttribute('data-theme', theme);
  } catch (e) {
    document.documentElement.setAttribute('data-theme', 'miami-night');
  }
})();
```

**Behavioral Guarantees**:
- Zero flash of wrong theme on initial visit, cross-page navigation, or hard reload (SC-002).
- First-time visitors always enter `miami-night` regardless of OS `prefers-color-scheme` (FR-007).
- Blocked `localStorage` (private browsing / strict tracking prevention) never throws unhandled errors.

---

## 2. Centralized CSS Custom Property Contract (`src/styles/tokens.css`)

All hex/RGB color definitions live **exclusively** inside `src/styles/tokens.css`. Component `.jsx` files and page `.jsx` files MUST contain **zero** hardcoded `#hex` strings or Tailwind arbitrary hex utilities (e.g., `bg-[#...]`, `from-[#...]`).

### Verified Token Palette & WCAG 2.1 AA Contrast Matrix

| Semantic Token | `miami-night` (Default) | Night Contrast | `pastel-day` | Day Contrast | WCAG 2.1 AA Gate |
|---|---|---|---|---|---|
| `--mv-bg-canvas` | `#120824` (Midnight Purple) | — | `#FFF6FA` (Pastel Blush Cream) | — | Base canvas |
| `--mv-bg-surface` | `#1B0E36` (Deep Synth Indigo) | — | `#FFFFFF` (Art Deco White) | — | Card / table surface |
| `--mv-bg-elevated` | `#26144A` (Arcade Plum) | — | `#F4EAFF` (Pastel Lavender) | — | Raised panel / header |
| `--mv-bg-header` | `#0C041A` (Deep Midnight) | — | `#FFF0F7` (Flamingo Mist) | — | Chrome surface |
| `--mv-ink-primary` | `#F8F5FF` | `17.95:1` (canvas) / `16.77:1` (surface) | `#1B0E33` | `17.12:1` (canvas) / `18.15:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-ink-muted` | `#D4C6F0` | `12.10:1` (canvas) / `11.30:1` (surface) | `#4E386E` | `9.37:1` (canvas) / `9.93:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-border-hot` | `#FF2A85` (Hot Magenta) | `5.45:1` (canvas) / `5.09:1` (surface) | `#D40072` (Flamingo Border) | `4.90:1` (canvas) / `5.19:1` (surface) | ≥ `3.0:1` PASS |
| `--mv-accent-cyan` | `#00F0FF` (Electric Cyan) | `13.73:1` (canvas) / `12.82:1` (surface) | `#006678` (Biscayne Deep Cyan) | `6.25:1` (canvas) / `6.62:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-accent-magenta-text` | `#FF6BB5` | `7.39:1` (canvas) / `6.90:1` (surface) | `#A80058` | `7.06:1` (canvas) / `7.48:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-focus-ring` | `#00F0FF` | `13.73:1` | `#A80058` | `7.06:1` | ≥ `3.0:1` PASS |
| `--mv-screen-1-border` | `#FF2A85` (Hot Magenta) | `5.09:1` (surface) | `#D40072` (Flamingo Neon) | `5.19:1` (surface) | ≥ `3.0:1` PASS |
| `--mv-screen-1-text` | `#FF6BB5` | `6.90:1` (surface) | `#A80058` | `7.48:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-screen-2-border` | `#00F0FF` (Electric Cyan) | `12.82:1` (surface) | `#00889E` (Biscayne Teal) | `4.19:1` (surface) | ≥ `3.0:1` PASS |
| `--mv-screen-2-text` | `#00F0FF` | `12.82:1` (surface) | `#006678` | `6.62:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-screen-3-border` | `#FFB800` (Solar Sunset Gold) | `10.41:1` (surface) | `#7C3AED` (Ocean Drive Violet) | `5.70:1` (surface) | ≥ `3.0:1` PASS |
| `--mv-screen-3-text` | `#FFB800` | `10.41:1` (surface) | `#5B21B6` | `8.98:1` (surface) | ≥ `4.5:1` PASS |
| `--mv-headliner-bg` | `#00F0FF` | — | `#A80058` | — | Badge fill |
| `--mv-headliner-ink` | `#120824` | `13.73:1` on `--mv-headliner-bg` | `#FFFFFF` | `7.48:1` on `--mv-headliner-bg` | ≥ `4.5:1` PASS |
| `--mv-vcr-bg` | `#0D051A` | — | `#1A0D2E` | — | Dark VCR display bezel |
| `--mv-vcr-digit` | `#39FF88` | `14.58:1` on `--mv-vcr-bg` | `#00F0FF` | `13.07:1` on `--mv-vcr-bg` | ≥ `4.5:1` PASS |

Additional visual effect tokens defined in `src/styles/tokens.css`:
- Sunset gradient stops: `--mv-sun-top`, `--mv-sun-mid`, `--mv-sun-bottom`, `--mv-glow-sun`
- Grid floor tokens: `--mv-grid-line`, `--mv-grid-glow`, `--mv-horizon-glow`
- Duotone tokens: `--mv-duotone-highlight` (cyan channel), `--mv-duotone-shadow` (magenta/purple channel)
- Elevation & neon shadows: `--mv-glow-magenta`, `--mv-glow-cyan`, `--mv-glow-screen-1`, `--mv-glow-screen-2`, `--mv-glow-screen-3`, `--mv-vcr-bezel-shadow`

---

## 3. Component Behavioral & Accessibility Contracts

### 3.1 `ThemeSwitcher` (`src/components/ThemeSwitcher.jsx` consumed by `Header.jsx`)
- **Semantic Element**: `<button type="button">` (or segmented control with semantic buttons) displaying **Miami Night** and **Pastel Day** mode indicators.
- **ARIA & Keyboard**:
  - Operable via `Tab`, `Enter`, and `Space` with a `2px` high-contrast `focus-visible` outline (`--mv-focus-ring`).
  - Exposes clear `aria-label` (e.g., `"Theme: Miami Night active. Switch to Pastel Day"` / `"Theme: Pastel Day active. Switch to Miami Night"`) and `aria-pressed` state so assistive technology announces the active mode immediately.
- **Feedback**: Updates `document.documentElement.dataset.theme` synchronously on click (`<100ms`), visually highlighting the active mode pill.

### 3.2 Synthwave Hero (`src/components/Hero.jsx` + pure CSS classes)
- **Content**: Displays `"Dan Beglins Mega Brum Tech Fest"` and primary tagline `"Better than tech mids"`.
- **Visual Layers** (`aria-hidden="true"` decorative layers behind semantic `<h1>` and CTA):
  - `.mv-sunset`: Pure CSS horizontal-striped circular sunset using `linear-gradient` + horizontal band `mask-image` / `-webkit-mask-image`.
  - `.mv-grid-floor`: Pure CSS `perspective()` + `rotateX()` horizon grid inside an `overflow-hidden` stage (zero horizontal scrollbar at `320px`).
- **Reduced Motion**: `@media (prefers-reduced-motion: reduce)` disables grid keyframe motion and glow transitions completely.

### 3.3 Speaker Cards & Detail (`Speakers.jsx`, `Speaker.jsx`)
- **Duotone Portrait**: `.mv-duotone-frame` applies CSS `grayscale(100%) contrast(1.15)` + `mix-blend-mode: multiply` over `--mv-duotone-highlight` and `::after` `mix-blend-mode: screen` with `--mv-duotone-shadow`.
- **Hover & Keyboard Focus Parity**: Snaps to full natural color (`filter: none; mix-blend-mode: normal; ::after { opacity: 0 }`) on both pointer `:hover` and keyboard `:focus-visible` / `:focus-within`.
- **Loading & Error States**: Fixed aspect-ratio frame prevents CLS; image `onError` renders a themed retro SVG monogram fallback (`initials` of speaker name) inside the duotone frame.
- **Headliner Badge**: Renders `<span className="mv-badge-headliner">HEADLINER</span>` exclusively when `speaker.sessionType === 'Keynote'`.

### 3.4 VHS / Arcade 3-Screen Schedule & VCR LED Counter (`Schedule.jsx`)
- **Default View & Column Order**:
  - Defaults `view` state to `'combined'` (unless opened with a shared `#p=` / `#picks=` URL hash, which opens `'myday'`).
  - Orders tracks strictly as **Screen 1 → Screen 2 → Screen 3** across `CombinedScheduleView`, `StackedScheduleView`, and `MyDayView`.
- **Distinct Neon Borders**:
  - `screen1` cells/headers/cards apply `--mv-screen-1-border` & `--mv-screen-1-text`.
  - `screen2` cells/headers/cards apply `--mv-screen-2-border` & `--mv-screen-2-text`.
  - `screen3` cells/headers/cards apply `--mv-screen-3-border` & `--mv-screen-3-text`.
- **VCR LED Counter (`.mv-vcr-counter`)**:
  - Renders inside the `"My day"` view switcher button as a recessed digital display showing zero-padded tabular digits (`00/08` .. `08/08`).
  - Includes screen-reader accessible text (`My day, {pickedCount} of {pickable.length} sessions picked`) while marking decorative separators/formatting appropriately.
- **Explicit Empty / Error / Success States**:
  - `00/08` empty state in `"My day"` prompts `"Pick one"` per time window, disables `Share my picks` and `Clear picks`, and explains local device persistence.
  - Clipboard/Web Share success announces `"Link copied"` inside `role="status"`; clipboard failure renders the full shareable URL inline for manual copy.

