Establish the foundational governing principles for this frontend UI project. Every subsequent specification, technical plan, and task list must strictly adhere to these rules:
1. UI Architecture & Component Design
Component Modularity: Break the interface into small, single-responsibility, reusable UI components. Keep presentation components pure and decoupled from data-fetching or state logic.
Simplicity First (YAGNI): Use native browser APIs, semantic HTML, and lightweight styling before reaching for heavy external UI libraries or complex state managers.
Responsive Mobile-First Layout: Every view must render cleanly across mobile, tablet, and desktop viewports using relative units and modern CSS layout primitives (Flexbox/Grid).
2. User Experience & Visual Consistency
Design Tokens Only: Never hardcode magic hex values, pixel spacing, or font sizes inside components; use centralized CSS variables or theme tokens.
Explicit UI States: Every interactive or asynchronous view must explicitly design for and handle loading, empty, error, and success states—never leave the user staring at a blank screen.
Instant Feedback: All user actions (clicks, form submissions, toggles) must provide immediate visual feedback (e.g., disabled states, spinners, or optimistic updates).
3. Accessibility (a11y)
Semantic HTML & Keyboard Navigation: Use proper semantic elements (<button>, <nav>, <main>, <label>) and ensure all interactive controls are fully operable via keyboard with visible focus indicators.
ARIA & Contrast: Enforce WCAG AA color contrast ratios and require descriptive aria-label or alt attributes on all icons, inputs, and media.
4. Code Quality & Testing
Strict Typing: Enforce strict static typing for all component props, events, and state models.
User-Centric Testing: Prioritize component and end-to-end tests that verify user-visible behavior (rendering, clicks, form validation) over testing internal implementation details.