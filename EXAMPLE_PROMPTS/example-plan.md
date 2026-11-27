/speckit.plan Create the technical implementation plan for spec.md adhering strictly to our UI constitution and these three architectural constraints:
1. Data & Schedule Integrity (speakers.js)
Modify speaker profiles, keynote flags, and session slot assignments exclusively inside speakers.js, preserving its existing object schema and export structure so the 3-screen schedule parser and /new preview route stay intact without breaking changes.
Generate static 80s retro SVG portrait assets in the public assets folder for Lucia Caminos & Jason Duval and Ken Rosenberg so they work natively with CSS duotone filters and full-color hover states.
2. Centralized Theme Tokens & Zero-Flash Bootstrapping
Define all colors, neon borders, surface elevations, and Screen 1 / Screen 2 / Screen 3 accent variables in a single root CSS design-token layer using data-theme="miami-night" (default) and data-theme="pastel-day" attributes—banning hardcoded hex values inside component files.
Inject a lightweight inline blocking script in the document <head> to read localStorage and apply data-theme before first paint so switching or reloading never flashes the wrong theme.
3. Pure CSS Retro Visuals & Reduced-Motion Guardrails
Build the synthwave striped sunset, perspective grid floor, magenta/cyan duotone image treatment, and VCR LED counter styling using native CSS gradients, blend modes, and box-shadow—do not install external animation or UI libraries.
Wrap all perspective grid motion and glow transitions in @media (prefers-reduced-motion: reduce) and validate WCAG AA contrast tokens across both themes.