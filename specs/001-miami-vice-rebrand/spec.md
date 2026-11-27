# Feature Specification: 80s Miami Vice / New Wave Conference Rebrand & Visual Overhaul

**Feature Branch**: `001-miami-vice-rebrand`

**Created**: 2026-11-27

**Status**: Draft

**Input**: User description: "Overhaul the conference website into an 80s Miami Vice / New Wave retro theme and rebrand the event:
1. Event Rebranding
Conference Name: Rename the event across the entire site (header, hero, schedule welcome banner, footer, and page metadata) to Dan Beglins Mega Brum Tech Fest.
Tagline: Update the primary hero tagline to "Better than tech mids".
2. Speaker Lineup Updates
Replace the existing Keynote speakers and add featured speakers to the lineup:
Dan Beglin — Keynote: "Why Mega Brum Tech Fest Beats Everything Else" (Role: Founder & Chief Synthwave Architect)
Lucia Caminos & Jason Duval - Talk: "Pair Programming Under Pressure: Shipping Fast When the Heat Is On" (Role: Staff Co-Founders @ Leonida Systems)
Ken Rosenberg — Talk: "Don't Panic! Survive P1 Production Outages Without Losing Your License" (Principal Incident & Compliance Counsel @ Rosenberg & Associates)
Retain the remaining community talks so the 3-screen schedule remains full.
3. Miami Vice / New Wave Visual Overhaul
Theme Switcher: Replace the standard Dark Mode toggle with a "Miami Night / Pastel Day" switcher (defaulting to Miami Night: midnight purple backgrounds, hot magenta borders, and electric cyan highlights).
Synthwave Hero: Feature a retro perspective grid floor and horizontal-striped sunset backdrop behind the new event title and tagline.
80s Speaker Cards: Apply a magenta/cyan duotone photo treatment to speaker headshots (snapping to full color on hover) and style Keynote speakers with a neon "HEADLINER" badge.
VHS / Arcade 3-Screen Schedule: Style Screen 1, Screen 2, and Screen 3 with distinct neon border colors and give the "My day" session picker a retro VCR LED counter look."

## Clarifications

### Session 2026-11-27

- Q: Which surface should the Miami Vice visual overhaul and rebrand target (`/` live site vs. `/new` preview)? → A: Overhaul the primary live site at `/` (shared content stays compatible; `/new` preview is out of visual styling scope).
- Q: How should the 3-screen schedule accommodate Dan Beglin's Keynote and the two new featured talks while keeping community talks? → A: Place Dan Beglin's Keynote in Simon Stevens's `09:45` slot (`Screen 3`), fill Dan's vacated `16:30` slot (`Screen 1`), and replace Mikolaj Maciejak's corporate sponsor slot (`Screen 2, 11:15`), keeping all 24 slots and independent community talks intact.
- Q: How should co-speakers Lucia Caminos & Jason Duval and missing speaker portraits be represented? → A: Model Lucia & Jason as one combined speaker card/page, using custom full-color 80s retro portrait illustrations for both new entries (duo + Ken Rosenberg).
- Q: How thoroughly should graphical logos, organizer attribution, and copyright text be rebranded in header, hero, and footer? → A: Replace all header, hero, and footer graphical logos, organizer credits, and copyright text with Dan Beglins Mega Brum Tech Fest synthwave branding (retaining historical names only inside Past Events archive entries).
- Q: What default view and screen order should visitors see first on the schedule? → A: Default to the Combined 3-screen view ordered Screen 1 → Screen 2 → Screen 3 (retaining Stacked and My day toggles).

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Experience Rebranded Event & Synthwave Hero Identity (Priority: P1)

Prospective attendees and community members arriving on any page of the conference website immediately recognize the event as **Dan Beglins Mega Brum Tech Fest** with the bold hero tagline **"Better than tech mids"** set against an unmistakable 1980s synthwave horizon featuring a horizontal-striped sunset and a perspective grid floor.

**Why this priority**: Establishes the core identity, positioning, and visual first impression of the conference across all entry points, navigation, and shared social previews.

**Independent Test**: Open the homepage, schedule page, speaker pages, and supporting informational pages; verify that the header, hero section, schedule welcome banner, footer, and browser/social page titles display "Dan Beglins Mega Brum Tech Fest", and that the homepage hero showcases "Better than tech mids" above a retro perspective grid floor and horizontal-striped sunset backdrop.

**Acceptance Scenarios**:

1. **Given** a visitor lands on the homepage, **When** the hero section loads, **Then** they see the event name "Dan Beglins Mega Brum Tech Fest", the primary tagline "Better than tech mids", a horizontal-striped sunset graphic, and a retro perspective grid floor behind the headline content.
2. **Given** a visitor navigates across any public page of the site (home, schedule, speakers, speaker detail, code of conduct, past events, convince-your-boss), **When** they inspect the site header, footer, schedule welcome banner, and browser tab title/metadata, **Then** every current-edition event title reference reads "Dan Beglins Mega Brum Tech Fest" with no leftover "TechMids Conf" branding for the current event.
3. **Given** a visitor views the hero on mobile, tablet, or desktop screens, **When** the viewport resizes down to narrow mobile widths, **Then** the event title, tagline, sunset backdrop, and perspective grid remain legible and proportional without horizontal page scrolling.

---

### User Story 2 - Switch Between Miami Night and Pastel Day Themes (Priority: P1)

Visitors experience the website in the signature **Miami Night** palette by default—midnight purple backgrounds, hot magenta borders, and electric cyan highlights—and can switch at any time to **Pastel Day** for bright daytime viewing, with instant visual confirmation and remembered preference across pages.

**Why this priority**: The site-wide theme governs readability, atmosphere, and contrast on every single page and component, making it foundational to the entire retro overhaul.

**Independent Test**: Load the website in a fresh browser session to confirm "Miami Night" is active by default, activate the "Miami Night / Pastel Day" switcher in the header via mouse and keyboard, verify immediate palette changes and accessible contrast in both modes, and navigate to another page or reload to confirm the chosen mode persists.

**Acceptance Scenarios**:

1. **Given** a first-time visitor with no saved theme preference opens any page, **When** the page renders, **Then** the **Miami Night** theme is active by default (displaying midnight purple backgrounds, hot magenta borders, and electric cyan highlights) regardless of operating system dark/light preference.
2. **Given** a visitor is viewing the site in Miami Night mode, **When** they activate the "Miami Night / Pastel Day" switcher, **Then** the switcher immediately updates its active state and transitions the site to the **Pastel Day** palette while maintaining readable text contrast across all sections.
3. **Given** a visitor has selected Pastel Day (or switched back to Miami Night), **When** they navigate to another page or refresh the browser, **Then** their selected theme remains active without flashing the unselected theme on load.
4. **Given** a keyboard or screen-reader user focuses the theme switcher, **When** they press Enter or Space, **Then** a visible focus ring surrounds the control, the theme toggles immediately, and assistive technology announces the current theme selection clearly.

---

### User Story 3 - Browse Updated Speaker Lineup & 80s Headliner Cards (Priority: P2)

Attendees exploring the speaker lineup discover the new featured speakers—headlined by Dan Beglin's Keynote alongside featured talks by Lucia Caminos & Jason Duval and Ken Rosenberg—presented alongside community speakers on 80s-styled cards with magenta/cyan duotone headshots that snap to full color on hover or focus, and a glowing neon **HEADLINER** badge on Keynote speakers.

**Why this priority**: Speakers and talk topics drive ticket sales and session attendance; showcasing the updated lineup and visual treatment turns the speaker directory into a centerpiece of the rebrand.

**Independent Test**: Navigate to the speakers section on the homepage and the dedicated speakers directory/detail pages; verify the three new/updated speaker entries and removed prior keynote, confirm Keynote cards display a neon "HEADLINER" badge, and hover/focus speaker cards to observe headshots snapping from magenta/cyan duotone to full color.

**Acceptance Scenarios**:

1. **Given** a visitor views the speaker lineup, **When** they inspect Keynote speakers, **Then** prior Keynote speakers are replaced by **Dan Beglin** presenting *"Why Mega Brum Tech Fest Beats Everything Else"* with role *"Founder & Chief Synthwave Architect"* and a prominent neon **HEADLINER** badge.
2. **Given** a visitor browses the full speaker lineup, **When** they look for featured talks, **Then** they see **Lucia Caminos & Jason Duval** (*"Pair Programming Under Pressure: Shipping Fast When the Heat Is On"*, role *"Staff Co-Founders @ Leonida Systems"*) and **Ken Rosenberg** (*"Don't Panic! Survive P1 Production Outages Without Losing Your License"*, role *"Principal Incident & Compliance Counsel @ Rosenberg & Associates"*), alongside the retained community speakers.
3. **Given** a visitor views any speaker card at rest, **When** they move their pointer over the card or focus the card link via keyboard, **Then** the speaker's headshot snaps from a magenta/cyan duotone treatment to full natural color and returns to duotone when pointer/focus leaves.
4. **Given** a visitor clicks any of the newly added or updated speakers, **When** the speaker detail page opens, **Then** the speaker's name, role, talk title, session badge ("HEADLINER" for Keynote, "Talk" for standard talks), scheduled screen/time, and talk description display accurately.

---

### User Story 4 - Plan Attendance on the VHS / Arcade 3-Screen Schedule (Priority: P2)

Attendees browsing the conference schedule see all three cinema tracks (**Screen 1**, **Screen 2**, and **Screen 3**) completely filled with sessions and visually distinguished by dedicated neon border colors, while tracking their personal session selections through a **"My day"** picker styled like a retro VCR LED counter.

**Why this priority**: Attendees rely on the multi-track schedule and personal itinerary builder before and during the event to navigate between the three screens without missing talks.

**Independent Test**: Open the schedule view, verify all time slots across Screen 1, Screen 2, and Screen 3 are populated and display distinct neon border colors per screen, verify the welcome banner greets attendees to Dan Beglins Mega Brum Tech Fest, and pick/unpick talks to confirm the "My day" VCR LED counter updates immediately.

**Acceptance Scenarios**:

1. **Given** a visitor opens the schedule section or page, **When** it loads, **Then** it defaults to the **Combined** 3-screen view ordered **Screen 1**, **Screen 2**, and **Screen 3** (retaining **Stacked** and **My day** view toggles), displaying each screen's distinct neon border color consistently across track headers, indicators, and session cards, with every session window across all three screens populated.
2. **Given** a visitor inspects the opening welcome slot on the schedule, **When** they read the welcome banner description, **Then** it welcomes attendees to **Dan Beglins Mega Brum Tech Fest**.
3. **Given** a visitor views the schedule controls, **When** they look at the **"My day"** session picker tab/control, **Then** the picked-session count (e.g., `00/08` or `0/8` format) is styled with a retro VCR LED digital counter aesthetic that remains legible in both Miami Night and Pastel Day themes.
4. **Given** a visitor picks or removes talks across time slots, **When** they click a session's Pick button, **Then** the session immediately shows its picked state, competing talks in that slot visually recede, and the VCR LED counter updates instantly to reflect the new count.

---

### Edge Cases & Explicit UI States *(mandatory per Constitution)*

- **Loading State**: Initial page paint applies the active theme ("Miami Night" by default or the visitor's saved choice) immediately so visitors never see an unstyled flash or jarring theme jump. Speaker headshots reserve fixed aspect-ratio frames with themed retro surface placeholders while images download so cards never shift layout.
- **Empty State**:
  - When an attendee opens **"My day"** with zero talks selected, the VCR LED counter displays zero picked slots, each time slot clearly prompts the attendee to choose one of the three screen options, and the "Share my picks" / "Clear picks" actions appear disabled with clear status text explaining that picks are saved on their device once selected.
  - If the speaker list or schedule data is ever empty, a themed message informs visitors that the lineup is being finalized and links to past events rather than leaving a blank section.
- **Error State**:
  - If a speaker portrait fails to load or no photograph is available, the card displays a stylized retro duotone-compatible avatar/monogram placeholder rather than a broken image icon.
  - If native sharing or clipboard copying fails when sharing "My day" picks, the full shareable URL is displayed inline in a status region so the attendee can copy it manually.
  - If device storage is unavailable or blocked (such as strict private browsing), switching between Miami Night and Pastel Day and picking talks in "My day" continue to work seamlessly for the active page session without error dialogs or crashes.
- **Success & Instant Feedback**:
  - Activating the "Miami Night / Pastel Day" switcher provides immediate toggle feedback (updated pressed state, active label highlight, and instant palette change).
  - Picking or unpicking a talk immediately toggles the button state ("Pick" vs. "Picked"), highlights the chosen card's neon border, dims unselected options in the same slot, and updates the VCR LED counter without delay.
  - Copying a "My day" share link immediately displays an inline confirmation message ("Link copied").
- **Boundary & Viewport Conditions**:
  - **Mobile (320px–639px)**: Single-column hero and stacked schedule cards, compact 1–2 column speaker grid, wrapping header navigation and theme switcher, and zero horizontal overflow. Touch users can tap/focus speaker cards without duotone styling obscuring faces or badges.
  - **Tablet (640px–1023px)**: Balanced 2–3 column speaker grid and clean track filter tabs with visible VCR LED counter.
  - **Desktop (1024px+)**: Side-by-side 3-screen schedule layout with distinct neon column/card borders, 4-column speaker showcase, and full-width synthwave sunset and perspective grid hero.
  - **Long Text & Co-Speakers**: Joint speaker names (*"Lucia Caminos & Jason Duval"*), lengthy titles (*"Principal Incident & Compliance Counsel @ Rosenberg & Associates"*), and multi-line talk titles wrap cleanly within cards without overflowing neon borders or overlapping badges.
  - **Reduced Motion**: Visitors with operating-system reduced-motion preferences enabled see crisp static synthwave grid/sunset artwork and instant color transitions without animated motion effects.

## Requirements *(mandatory)*

### Functional, UX & Accessibility Requirements

- **FR-001**: The primary public website (`/` and its canonical subpages) MUST display **Dan Beglins Mega Brum Tech Fest** across all current-event text and graphical branding touchpoints—replacing legacy graphical logos/wordmarks, organizer attribution, and copyright notices in the site header, homepage hero, schedule welcome banner, site footer, structured event metadata, and page titles/descriptions (retaining historical names only inside Past Events archive entries); shared content data MUST remain compatible with the unpublished `/new` preview, which is excluded from visual styling changes.
- **FR-002**: The homepage hero MUST display **"Better than tech mids"** as the primary hero tagline alongside the rebranded conference name.
- **FR-003**: The speaker lineup MUST replace all existing Keynote speaker entries with **Dan Beglin** presenting the Keynote *"Why Mega Brum Tech Fest Beats Everything Else"* with the role *"Founder & Chief Synthwave Architect"*.
- **FR-004**: The speaker lineup MUST include **Lucia Caminos & Jason Duval** modeled as a single combined co-speaker profile/card presenting the Talk *"Pair Programming Under Pressure: Shipping Fast When the Heat Is On"* with the role *"Staff Co-Founders @ Leonida Systems"*.
- **FR-005**: The speaker lineup MUST include **Ken Rosenberg** presenting the Talk *"Don't Panic! Survive P1 Production Outages Without Losing Your License"* with the role *"Principal Incident & Compliance Counsel @ Rosenberg & Associates"*.
- **FR-006**: The 3-screen conference schedule MUST place Dan Beglin's Keynote in the `09:45–10:30` `Screen 3` slot (replacing Simon Stevens) and assign Lucia Caminos & Jason Duval's Talk and Ken Rosenberg's Talk to Dan Beglin's vacated `16:30–17:15` `Screen 1` slot and Mikolaj Maciejak's `11:15–12:00` `Screen 2` corporate sponsor slot (removing Simon Stevens and Mikolaj Maciejak from the speaker lineup), keeping all 21 remaining community talks and all 24 total session slots populated with zero duplicate speaker appearances.
- **FR-007**: The site header MUST replace the legacy Dark Mode toggle with a **"Miami Night / Pastel Day"** theme switcher that defaults to **Miami Night** (midnight purple backgrounds, hot magenta borders, and electric cyan highlights) for first-time visitors and remembers the visitor's chosen theme across page loads on the same device.
- **FR-008**: Selecting **Pastel Day** on the theme switcher MUST apply a coordinated, daylight-friendly 1980s Miami pastel palette across all pages and components while preserving unmistakable retro visual identity and readable contrast.
- **FR-009**: The homepage hero section MUST render a retro synthwave backdrop featuring both a **horizontal-striped sunset** and a **perspective grid floor** behind the event title and primary tagline, adapting cleanly to both Miami Night and Pastel Day themes.
- **FR-010**: All speaker cards MUST render speaker headshots—including custom full-color 80s retro portrait illustrations for newly added speakers without photographs (Lucia Caminos & Jason Duval, and Ken Rosenberg)—with a **magenta/cyan duotone** visual treatment at rest and snap to **full color** when hovered by a pointer or focused via keyboard.
- **FR-011**: Speaker cards and session badges for Keynote speakers MUST display a prominent neon **"HEADLINER"** badge distinguishing them from standard talks.
- **FR-012**: The schedule MUST open by default in the **Combined** 3-screen view ordered **Screen 1 → Screen 2 → Screen 3** (while retaining **Stacked** and **My day** view toggles) and style **Screen 1**, **Screen 2**, and **Screen 3** with three **distinct neon border colors** applied consistently to each screen's track indicators, headers, and session cards across all schedule views.
- **FR-013**: The **"My day"** session picker control MUST display the attendee's picked-session count (picked slots out of total pickable slots) styled as a **retro VCR LED counter** (segmented/digital glowing readout appearance) that updates immediately as talks are picked or cleared.
- **FR-014**: All visual colors, neon accents, surfaces, typography scales, and shadows for both Miami Night and Pastel Day MUST be governed by centralized theme design tokens so visual styling remains consistent site-wide with zero ad-hoc one-off color values.
- **FR-015**: Every page and updated component MUST render responsively mobile-first across mobile, tablet, and desktop viewports without horizontal overflow, provide immediate visual feedback on every user interaction, and explicitly handle loading, empty, error, and success states.
- **FR-016**: All text, badges (including the neon "HEADLINER" badge), VCR LED counter digits, track indicators, and interactive controls MUST meet or exceed **WCAG 2.1 Level AA** color contrast ratios (at least 4.5:1 for normal text and 3:1 for large text and UI boundaries) in both **Miami Night** and **Pastel Day** modes, support full keyboard navigation with visible focus indicators, and expose descriptive accessible names and state announcements to screen readers.

### Key Entities *(include if feature involves data)*

- **Event Identity**: Represents the conference edition metadata—conference name (*"Dan Beglins Mega Brum Tech Fest"*), primary tagline (*"Better than tech mids"*), event date, venue details, and social/search page descriptions.
- **Speaker & Session Entry**: Represents an individual speaker or co-speaking pair, including display name, role/company title, portrait image (or retro avatar artwork), biography, talk title, talk abstract, and session type (*Keynote / HEADLINER* vs. *Talk*).
- **Schedule Screen (Track)**: Represents one of the three cinema rooms (*Screen 1*, *Screen 2*, *Screen 3*), each associated with its own distinct neon accent/border token and ordered list of time slots.
- **Attendee Preferences**: Represents client-side visitor choices persisted on their device, comprising the active visual theme (*Miami Night* default vs. *Pastel Day*) and the list of selected session IDs in *"My day"* driving the VCR LED counter.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 100% of public pages and shared layout regions (header, homepage hero, schedule welcome banner, footer, and page metadata) display **"Dan Beglins Mega Brum Tech Fest"** with zero remaining current-edition references to "TechMids Conf", and the homepage hero prominently displays **"Better than tech mids"**.
- **SC-002**: 100% of first-time visits load directly into the **Miami Night** theme with zero unstyled or wrong-theme flashes, and switching between **Miami Night** and **Pastel Day** updates the entire viewport in under 100 milliseconds while persisting across page reloads.
- **SC-003**: All 3 new/updated featured speaker sessions (Dan Beglin as Headliner Keynote, Lucia Caminos & Jason Duval, and Ken Rosenberg) are discoverable in both the speaker lineup and the 3-screen schedule, and 100% of the 24 talk slots across the 8 session windows and 3 screens remain filled with zero empty slots or scheduling conflicts.
- **SC-004**: 100% of speaker cards display the magenta/cyan duotone portrait treatment at rest, reveal full color immediately on hover and keyboard focus, and display the neon **"HEADLINER"** badge exclusively on Keynote entries.
- **SC-005**: Screen 1, Screen 2, and Screen 3 are visually distinguishable at a glance via three distinct neon border colors in schedule views, and picking or unpicking any talk updates the **"My day"** VCR LED counter immediately in 100% of interactions.
- **SC-006**: 100% of text, neon badges, VCR LED counter readouts, and interactive controls pass **WCAG 2.1 Level AA** contrast thresholds in both **Miami Night** and **Pastel Day** themes, and all interactive journeys (theme switching, speaker browsing, schedule filtering, and "My day" session picking) are 100% completable using keyboard navigation alone.
- **SC-007**: All pages render cleanly with zero horizontal page overflow across viewport widths from 320px mobile screens through 1440px+ desktop monitors.

## Assumptions

- **Primary Live Site Scope**: Visual overhaul and rebranding UI changes apply exclusively to the primary public conference experience at `/` (and its canonical subpages), while shared event, speaker, and schedule data remain compatible with `/new` without applying visual styling changes to the unpublished `/new` preview.
- **Exact Rebrand Copy & Wordmarks**: The conference name uses the exact spelling requested—**"Dan Beglins Mega Brum Tech Fest"**—and the primary hero tagline uses **"Better than tech mids"**. All header, hero, and footer graphical logos, organizer credits, and copyright lines are replaced with synthwave **Dan Beglins Mega Brum Tech Fest** branding, while historical references inside "Past Events" archive entries retain their original historical names.
- **Schedule Slot Allocation**: Simon Stevens (`Screen 3, 09:45–10:30`) and corporate sponsor speaker Mikolaj Maciejak (`Screen 2, 11:15–12:00`) are replaced; Dan Beglin moves from `Screen 1, 16:30–17:15` into the `Screen 3, 09:45–10:30` Keynote slot, and the two new featured talks (Lucia Caminos & Jason Duval, and Ken Rosenberg) occupy the `Screen 2, 11:15–12:00` and `Screen 1, 16:30–17:15` slots so all 24 session slots remain 100% full and every independent community talk is retained.
- **Joint Speaker Representation**: Co-speakers **Lucia Caminos & Jason Duval** are presented together as a single joint speaker/session profile so their shared talk appears cohesively in the speaker grid, speaker detail page, and schedule card.
- **Speaker Portrait Assets**: Dan Beglin continues to use his existing speaker photograph, while custom full-color 80s retro portrait illustrations are created for **Lucia Caminos & Jason Duval** (duo portrait) and **Ken Rosenberg** so every card exhibits a genuine magenta/cyan duotone-to-full-color transition on hover and focus.
- **Unchanged Event Logistics**: Conference date (27th November 2026), venue (Everyman Cinema, Mailbox, Birmingham), ticket purchasing flow, sponsors, and community meetup partners remain unchanged unless directly referencing the conference title.

