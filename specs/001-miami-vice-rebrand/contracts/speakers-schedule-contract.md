# Contract: `speakers.js`, `schedule.js`, & Portrait Asset Integrity

**Feature**: `001-miami-vice-rebrand`  
**Governs**: Architectural Constraint #1 (`speakers.js` single source of truth, `/new` route compatibility, static SVG portraits)

---

## 1. `speakers.js` Module Export Contract

- **File Path**: `speakers.js` (repository root — path and filename MUST NOT change)
- **Named Export**: `export const Sessions`
- **Key Count**: Exactly `24` enumerable own keys, each matching a URL-safe PascalCase `speakerId`.

### Required Per-Speaker Object Shape

```ts
export type ScreenId = 'screen1' | 'screen2' | 'screen3';
export type SessionType = 'Keynote' | 'Talk';

export interface StaticPortraitImport {
  src: string;
  width: number;
  height: number;
  blurDataURL?: string;
}

export interface ScheduleSlotAssignment {
  screen: ScreenId;
  start:
    | '09:45 AM'
    | '10:30 AM'
    | '11:15 AM'
    | '13:00 PM'
    | '13:45 PM'
    | '15:00 PM'
    | '15:45 PM'
    | '16:30 PM';
  end:
    | '10:30 AM'
    | '11:15 AM'
    | '12:00 PM'
    | '13:45 PM'
    | '14:30 PM'
    | '15:45 PM'
    | '16:30 PM'
    | '17:15 PM';
}

export interface SpeakerProfile {
  name: string;
  jobTitle: string;
  bio: JSX.Element;
  talkTitle: string;
  description: JSX.Element;
  image: StaticPortraitImport;
  sessionType: SessionType;
  slot: ScheduleSlotAssignment;
  speakerCard: boolean;
  speakerCardImage: string;
  website?: string;
  linkedin?: string;
  github?: string;
  x?: string;
  bluesky?: string;
  mastodon?: string;
  instagram?: string;
  youtube?: string;
  facebook?: string;
  devto?: string;
  tiktok?: string;
}

export type SessionsMap = Record<string, SpeakerProfile>;
```

### Non-Breaking Compatibility Rules
1. `sessionType` MUST remain `"Keynote"` for Keynote speakers and `"Talk"` for standard talks in `speakers.js`. The live `/` UI maps `sessionType === "Keynote"` to the neon `"HEADLINER"` visual badge, while `/new` (`src/new/SpeakerCard.jsx`) continues evaluating `speaker.sessionType === 'Keynote'` without modification.
2. `image` MUST be an imported static asset object (`import X from "./public/..."`), NEVER a bare string path, because:
   - `src/components/Schedule.jsx` (`StackedScheduleView`) passes `Sessions[speakerId].image` to `next/image` without `fill`, requiring `.width` and `.height` on the object.
   - `src/new/SpeakerCard.jsx`, `src/new/ScheduleBoard.jsx`, and `src/pages/new/speakers/[speaker].jsx` read `speaker.image.src`.
3. `slot` is an additive property on each `SpeakerProfile`; existing consumers that iterate `Object.keys(speaker)` for `SOCIALS` (`Speaker.jsx` line 120: `Object.keys(speaker).filter((key) => SOCIALS[key] && speaker[key])`) safely ignore `slot` because `SOCIALS['slot']` is `undefined`.

---

## 2. `schedule.js` Derived Export Contract

- **File Path**: `schedule.js` (repository root — path and filename MUST NOT change)
- **Dependency Rule**: Imports `{ Sessions } from "./speakers"`. MUST NOT import from `./event` (since `event.js` imports `{ OVERALL_END, OVERALL_START } from "schedule"`).
- **Named Exports**:
  - `export const OVERALL_START = '08:30AM'`
  - `export const OVERALL_END = '17:15PM'`
  - `export const Schedule = { conferenceSchedule: { day1 }, screen1: { day1 }, screen2: { day1 }, screen3: { day1 } }`
- **Welcome Slot**:
  - `name: 'Welcome'`
  - `description: 'Welcome to Dan Beglins Mega Brum Tech Fest!'`
  - `start: '09:30AM'`, `end: '09:45AM'`
- **Derived Talk Slot Entry Shape**:
  ```ts
  {
    name: string;        // Derived from Sessions[speakerId].name
    description: string; // Derived from Sessions[speakerId].talkTitle
    start: string;       // Derived from Sessions[speakerId].slot.start
    end: string;         // Derived from Sessions[speakerId].slot.end
    session: [string];   // [speakerId]
  }
  ```
- **Parser Compatibility Guarantee**:
  - `src/components/Schedule.jsx` (`buildRows`), `src/picks/usePicks.js` (`currentTalks`), and `src/new/data.js` (`buildRows`, `dayOutline`, `findTalk`, `moreSpeakersToCome`) all continue to receive the exact `Schedule[screenId].day1.timeSlots` structure with zero breaking changes.

---

## 3. Static 80s Retro SVG Portrait Asset Contract

| Speaker Key | Asset Path | Root SVG Attributes | Visual Composition Requirements |
|---|---|---|---|
| `LuciaCaminosJasonDuval` | `public/assets/lucia-jason.svg` | `xmlns="http://www.w3.org/2000/svg" width="800" height="800" viewBox="0 0 800 800"` | Full-color 1980s synthwave co-founder duo portrait (warm sunlit highlights, pastel/tropical Vice blazers, aviator/wayfarer sunglasses, palm silhouette & striped horizon backdrop) with wide luminance range (`L = 0.08` shadows to `L = 0.90` highlights) for crisp CSS duotone mapping |
| `KenRosenberg` | `public/assets/ken-rosenberg.svg` | `xmlns="http://www.w3.org/2000/svg" width="800" height="800" viewBox="0 0 800 800"` | Full-color 1980s Miami legal counsel portrait (white/pastel linen suit, patterned tie, oversized gold-rim glasses, expressive "don't panic" incident-counsel pose, Venetian blind neon office backdrop) with distinct shadow/midtone/highlight separation |

**Asset Constraints**:
- Self-contained static vector markup only (`<svg>`, `<defs>`, `<linearGradient>`, `<path>`, `<rect>`, `<circle>`, `<g>`).
- Zero external `<image href="...">`, zero `@import` web fonts inside SVG, and zero embedded scripts so SVGs render deterministically inside `<img>` tags across all browsers and static exports.

