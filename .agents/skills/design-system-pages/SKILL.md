---
name: design-system-pages
description: |
  Author and extend code-based design-system mockup pages: a single
  self-contained HTML page that renders a product's screens as React mockups
  on a Figma-like pan/zoom canvas, driven by in-code design tokens. Use when:
  (1) creating the design-system mockup page for a project, (2) adding a
  screen mockup for a new feature, (3) evolving design tokens or shared UI
  primitives, (4) verifying a UI implementation against approved mockups.
  Keywords: "design system", "mockup", "artboard", "design canvas", "design
  tokens", "screen design", "identity page", "visual direction", "design
  plan", "microcopy".
---

# Design System Pages

You author and maintain a project's design-system mockup page: **one
self-contained HTML file** that renders every product screen as a React
mockup on a Figma-like pan/zoom canvas. The page needs no build step, no
package install, and no assets — it runs by opening the file (or serving the
folder statically) with network access for CDN scripts and fonts. The mockup
page is the project's **visual source of truth**: implementation work builds
what the page shows, and the page wins over implementation on visual
disagreements unless project instructions say otherwise.

Every visual decision goes through a **design plan** first (see *Design plan
before code*) — never start components from a blank file.

## Locating the page in a project

Search the repository for the page before creating anything:

- An `.html` file whose scripts are `type="text/babel"` and that renders a
  canvas root — typically inside the project's design/docs folder.
- The companion `.jsx` files it loads (`canvas`, `tokens`, `ui`, screens).

If the project has no page yet, bootstrap one per **Bootstrap a new page**
below, in the folder where the project keeps design documentation (a
`design/` folder under its docs is a sound default).

## Page anatomy (the pattern)

| File | Role | Exports via `Object.assign(window, ...)` |
|---|---|---|
| `<Name> Identity.html` (or similar) | The page. Loads React 18 UMD + Babel standalone from CDN, the project fonts (Google Fonts), every script below, then defines an inline `App` composing the canvas and calls `ReactDOM.createRoot(...).render(<App/>)`. | — |
| `design-canvas.jsx` | Canvas infrastructure: `DesignCanvas` (transform-based pan/zoom viewport; trackpad pinch zooms, scroll pans, drag pans; zoom-at-cursor via `translate3d` written straight to the DOM), `DCSection` (title + subtitle + horizontal stack of artboards), `DCArtboard` (labeled card with width/height), `DCPostIt` (sticky note for annotations). | `DesignCanvas, DCSection, DCArtboard, DCPostIt` |
| `components/tokens.jsx` | `<PREFIX>_TOKENS` — one object with named directions (commonly two: a light and a dark, or two identity proposals), plus brand primitives `Mark` and `Wordmark`. | tokens + brand |
| `components/ui.jsx` | Shared primitives, all token-driven (take a `t` prop): button, badge, input, status, waveform/media placeholder, thumbnail, section label, and an inline SVG icon set. | all primitives + icon set |
| `components/<screen>.jsx` | One file per screen area (e.g. landing, auth, library, upload, detail, notifications). May hold several related components plus sample data constants. | the screen components |
| `scraps/` (optional) | Scratch sketches **and project design memory**: directions tried and rejected, so later passes don't repeat them. Never rendered. | — |

Script load order in the HTML matters: React UMD → Babel standalone →
`design-canvas.jsx` → `components/tokens.jsx` → `components/ui.jsx` → screen
files → the inline script defining `App`. Tokens and UI primitives come
before every screen file because screens consume them at module evaluation
time.

## Naming and prefix rule

Every project brands the page's API with a short uppercase prefix derived
from the product name (2–4 letters): `<PREFIX>_TOKENS`, `<PREFIX>Button`,
`<PREFIX>AppShell`, `<PREFIX>Wordmark`.

- **Existing page:** discover the prefix by reading the files — search for
  `_TOKENS =` and the `Object.assign(window` lines. **Never invent a new
  prefix when one exists**; reuse it exactly so component references stay
  stable.
- **New page:** derive the prefix from the product name and use it
  consistently everywhere (tokens, primitives, screens, sample data).

## Design plan before code

Work in two passes: plan, self-review, then code.

**Pass 1 — the plan.** Short, in chat or a scratch file (not a committed
design doc). Produce:

- **Color:** the core palette as 4–6 named values with roles (surface, ink,
  accent, status).
- **Type:** the families and their roles, plus a simple type scale (display,
  body, data/mono).
- **Layout:** a one-paragraph concept per screen and ASCII wireframes to
  compare options, with explicit alignment decisions.
- **Principles:** 2–3 sentences on what makes this page unique to this
  product — the thing a competitor's page could not have.
- **Boldness budget:** the single element allowed to be memorable; everything
  else stays quiet and disciplined.

**Pass 2 — self-review against genericity.** Ask: *would I produce this same
plan for any similar product?* Any part that would come out identical is a
default, not a choice — revise it and state what changed and why. Then run
the calibration checklist below over the revised plan. Only now write code,
following the revised plan.

### Calibration checklist (generic-design tells)

Generated pages cluster into these traits. They are legitimate for some
briefs — when the project's established direction or the product genuinely
calls for one, follow it deliberately — but never let a free axis spend
itself on a default:

1. Warm cream background near `#F4F1EA` + high-contrast serif display +
   terracotta/warm-clay accent near `#D97757`.
2. Near-black background with one bright acid-green or vermilion accent.
3. Broadsheet layout: hairline rules, zero border-radius, dense
   newspaper-like columns.
4. The SaaS-card kit: identical rounded cards everywhere, one border-radius
   regardless of hierarchy, the same soft grey shadow under each, gradient
   washes as decoration.
5. Template chrome independent of subject: tracked-out ALL-CAPS eyebrow
   label above every heading; meta strings joined with middle dots; "WORD —
   fragment" labels with spaced em dashes; tinted near-black (`#0B0B0B`,
   `#111`) standing in for black; a monospace face for every small data
   label; a `→` appended to links and buttons; a big-number-with-small-label
   hero by default; fade-and-slide-up entrances on each section; hover
   transitions on every card.

Also check: numbered markers (01 / 02 / 03) only when the content truly is a
sequence; no decorative accent on a single word of a headline; no all-caps
labels without informational function.

## Non-negotiable conventions

1. **One component, every direction.** Each screen component takes a single
   `t` prop (the token object for one direction) and must render correctly
   for **every** direction defined in `<PREFIX>_TOKENS`. Never write a screen
   that only works in one direction.
2. **Tokens only.** Colors, radii, shadows, and status tones come from the
   `t` object — never hardcoded in JSX. See `references/token-reference.md`
   for the canonical key groups.
3. **Inline styles, zero build.** Components use inline `style={{...}}` JSX.
   No CSS files, no bundler, no TypeScript, no imports. Each file defines
   plain top-level `function` components and registers its exports at the
   bottom with `Object.assign(window, {...})`.
4. **Canvas composition lives in the HTML.** `DCSection`, `DCArtboard`, and
   `DCPostIt` are used only inside the inline `App`, never inside screen
   components. Screens are pure product UI; the canvas is presentation
   around them.
5. **Real content, all states.** Screens carry realistic product content —
   real names, timestamps, states, quantities. No lorem ipsum, no placeholder
   microcopy (write copy per `references/design-guidelines.md`). Where a
   screen has meaningful data states, mock them: at minimum the loaded state,
   plus error and empty states when the feature can produce them.
6. **Restraint and motion.** Spend the direction's boldness budget in one
   place and cut decoration that serves nothing. One orchestrated,
   non-user-triggered moment per screen at most; motion that answers a user
   action (opening, confirming, expanding) is welcome — show what changed;
   scattered entrance effects are not.
7. **Editable surface.** If the inline script starts with a
   `TWEAKS = /*EDITMODE-BEGIN*/{...}/*EDITMODE-END*/` JSON block (accent hue
   per direction, a `showOnly` screen filter) and listens for
   `__activate_edit_mode` / `__deactivate_edit_mode` postMessages, keep that
   block and its markers intact — external editors drive it.

## Adding a screen mockup

1. **Plan, then write.** For a genuinely new screen area, run the
   design-plan passes scaled to the screen (concept, wireframe, principle).
   Then write the component in the most fitting existing file, or create
   `components/<screen>.jsx`:
   - Signature: `function <PREFIX><Name>Page({ t }) { ... }` — tokens in,
     JSX out.
   - Consume primitives from `ui.jsx` and app shells instead of rebuilding
     chrome; brand from `Mark`/`Wordmark`; icons from the shared set.
   - Real content and all meaningful states (loaded / error / empty), with
     copy written per `references/design-guidelines.md`.
   - Register exports: `Object.assign(window, { <PREFIX><Name>Page });`
2. **Register the script** in the HTML after `components/ui.jsx` (and after
   any file it depends on):
   `<script type="text/babel" src="components/<screen>.jsx"></script>`
3. **Wire a canvas section** in the inline `App`: a
   `{(showAll || show==='<name>') && (<DCSection title="0X · <Name>" ...>`
   block containing **one `DCArtboard` per token direction**, each rendering
   the screen with that direction's tokens (`t={tA}`, `t={tB}`). Use
   consistent artboard sizes per screen type (full pages tall, auth shorter,
   floating panels small).
4. **Verify both directions honestly.** Walk the screen through every token
   set; if contrast or overlap breaks in one direction, fix the component —
   never special-case a direction.
5. **Critique with screenshots.** Open the HTML (serve the folder statically
   or open the file directly; network access needed for CDN scripts and
   fonts). A blank page means a Babel compile error — check the browser
   console. Check: canvas pans/zooms, the new section shows one artboard per
   direction with correct labels, icons render, media placeholders look
   loaded, nothing clipped or overflowing. Then screenshot the artboards and
   critique like a reviewer: broken hierarchy, decoration that serves
   nothing, any calibration tell that slipped in. Fix everything in one
   batched pass, confirm with at most one more pass, and stop polishing.
   Record directions tried and rejected in `scraps/`.

## Bootstrap a new page

When the project has no mockup page yet:

1. Create the page folder where the project keeps design documentation
   (e.g. `<docs>/design/<product-slug>/`) with the anatomy files above.
2. **Produce the design plan** per *Design plan before code*, then write
   `<PREFIX>_TOKENS` with **two named directions** derived from the plan —
   each a complete key set: typically one light and one dark identity, or
   two competing identities — with `name` and `tagline`. Pick typefaces that
   fit the product's subject matter (one UI/display family + one mono for
   timestamps/metadata is a sound default), loaded from Google Fonts.
3. Copy or reimplement the canvas infrastructure (`design-canvas.jsx`): it is
   product-agnostic — pan/zoom viewport, sections, artboards, post-its.
4. Build shared primitives in `components/ui.jsx`, then the screens that the
   project's features need, one file per screen area.
5. Compose everything in the HTML's `App`: header strip introducing the
   identity, then one `DCSection` per screen with one artboard per direction,
   plus a token/type reference artboard.
6. Verify in a real browser and critique with screenshots per *Adding a
   screen mockup* steps 4–5.

## Quality bar

If the project defines a design rubric or evaluator agent (e.g. under its
agent configuration), apply it as the acceptance bar for new screens. In
absence of one, hold the bar at: a distinct, cohesive identity specific to
the product's subject matter — its materials, vernacular, and data shapes —
never generic template aesthetics (the calibration checklist is the floor,
not the target). Depth guides live in `references/`:

- `references/token-reference.md` — the canonical token key set and
  consumption rules.
- `references/design-guidelines.md` — microcopy, typography, motion, and the
  self-critique protocol.

## Relationship to implementation code

The mockup page is the reference implementation's design contract. When
product UI code and mockup disagree on visuals, the mockup wins unless
project instructions say otherwise. Keep screen component names and the
prefix stable — feature plans and specs commonly reference them by name, and
renaming breaks those references.
