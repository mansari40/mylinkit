---
noteId: "b1206610a7d111f1bcf2d70766d52b5b"
tags: []

---

# mylinkit — Design System

Token reference for `docs/design-system.md`. Companion to `ui.md` (components, layouts, accessibility).

Source of truth: `tailwind.config.js` + `globals.css`, as established across `design.md` and the built screens (onboarding, dashboard, public profile, landing). Where the system hasn't defined a value yet, this doc says **TBD** rather than inventing one.

---

## 1. Colors

Tailwind classes below use the custom keys already added to `tailwind.config.js` (`theme.extend.colors`), so `bg-accent`, `text-text-secondary`, `border-border` etc. all resolve through CSS variables — not the stock Tailwind palette.

### Product UI (light / dark)

| Token | Tailwind key | Light hex | Dark hex | Usage |
|---|---|---|---|---|
| Background | `bg` (`bg-bg`) | `#FFFFFF` | `#0E0E10` | App background, cards |
| Background subtle | `bg-subtle` (`bg-bg-subtle`) | `#F5F5F7` | `#1A1A1D` | Inputs, secondary buttons, sunken surfaces |
| Background muted | `bg-muted` (`bg-bg-muted`) | `#EDEDF2` | `#1F1F23` | Preview panel / phone-frame background |
| Surface | `surface` (`bg-surface`) | `#FFFFFF` | `#18181B` | Link cards, modals |
| Border | `border` (`border-border`) | `#E5E5EA` | `#2C2C31` | Hairlines, dividers |
| Text primary | `text-primary` (`text-text-primary`) | `#0A0A0A` | `#F5F5F7` | Headlines, body |
| Text secondary | `text-secondary` (`text-text-secondary`) | `#6B6B70` | `#9A9AA2` | Subtext, meta (click counts, helper text) |
| Text inverse | `text-inverse` (`text-text-inverse`) | `#FFFFFF` | `#0A0A0A` | Text on dark/accent surfaces |
| Accent | `accent` (`bg-accent` / `text-accent`) | `#05DA5B` | `#1FE871` | Primary buttons, active state, links |
| Accent hover | `accent-hover` (`bg-accent-hover`) | `#04C24F` | `#3FF285` | Accent hover/active |
| Inverse surface | `inverse-surface` (`bg-inverse-surface`) | `#0A0A0A` | `#F5F5F7` | Inverted bars, final-CTA section |

### Marketing palette (landing page only, flat full-bleed sections)

| Token | Tailwind key | Hex | Reference section |
|---|---|---|---|
| Lime | `marketing.lime` (`bg-marketing-lime`) | `#E8FF6E` | Promo bar |
| Blue | `marketing.blue` (`bg-marketing-blue`) | `#3D5AFE` | "Create and customize" section |
| Maroon | `marketing.maroon` (`bg-marketing-maroon`) | `#7A1F2B` | "Share your link" / testimonial / FAQ |
| Olive | `marketing.olive` (`bg-marketing-olive`) | `#D6DE8C` | Analytics stats section |
| Purple | `marketing.purple` (`bg-marketing-purple`) | `#4B2478` | Final CTA section |
| Pink | `marketing.pink` (`bg-marketing-pink`) | `#F3C6E0` | Feature card |
| Cream | `marketing.cream` (`bg-marketing-cream`) | `#FBFAF7` | Neutral content sections |

### Not yet in the config

| Token | Used where | Status |
|---|---|---|
| `--color-bg-page` | Public profile page backdrop (`app/[handle]`) | **TBD** — referenced with a CSS fallback (`#E2E2E7`) but never formally added to `globals.css` or `tailwind.config.js`. Add it or repoint to an existing token before shipping. |
| Error/destructive color | Delete action, "taken" handle state (`#E0455B`) | **TBD** — used ad hoc as a raw hex in two components, not yet a named token. Recommend adding `--color-danger`. |

---

## 2. Typography

One family throughout — **Plus Jakarta Sans** (open-source stand-in for the reference's Sofia Pro). Hierarchy comes from size and weight, not a second typeface.

```css
font-family: "Plus Jakarta Sans", ui-sans-serif, system-ui, sans-serif;
```

| Token | Size / line-height | Weight | Usage | Tailwind class |
|---|---|---|---|---|
| `display-xl` | 56px / 1.1 | 800 | Marketing hero headline | **TBD** — not yet added to `theme.extend.fontSize` |
| `display-lg` | 40px / 1.15 | 800 | Onboarding headline, section headlines | **TBD** |
| `display-md` | 28px / 1.2 | 700 | Editor panel headers, profile username | **TBD** |
| `heading` | 20px / 1.3 | 700 | Card titles, subsection headers | **TBD** |
| `body-lg` | 17px / 1.5 | 400 | Marketing body copy, onboarding subtext | **TBD** |
| `body` | 15px / 1.5 | 500 | Link card titles, button labels | **TBD** |
| `body-sm` | 13px / 1.4 | 400 | Meta text (click counts, domains), footer links | **TBD** |
| `caption` | 12px / 1.4 | 500 | Toggle/tab labels, small badges | **TBD** |

The values themselves are settled (used consistently across onboarding, dashboard, and public-profile code via literal `text-[Npx]` classes). What's missing is wiring them into `tailwind.config.js` as named `fontSize` keys so engineers use `text-display-lg` instead of `text-[40px]`. Recommended addition:

```js
// tailwind.config.js — theme.extend.fontSize (not yet added)
fontSize: {
  "display-xl": ["56px", { lineHeight: "1.1", fontWeight: "800" }],
  "display-lg": ["40px", { lineHeight: "1.15", fontWeight: "800" }],
  "display-md": ["28px", { lineHeight: "1.2", fontWeight: "700" }],
  heading: ["20px", { lineHeight: "1.3", fontWeight: "700" }],
  "body-lg": ["17px", { lineHeight: "1.5", fontWeight: "400" }],
  body: ["15px", { lineHeight: "1.5", fontWeight: "500" }],
  "body-sm": ["13px", { lineHeight: "1.4", fontWeight: "400" }],
  caption: ["12px", { lineHeight: "1.4", fontWeight: "500" }],
}
```

Rules observed in the built screens:
- No all-caps labels anywhere — sentence case throughout.
- Line length for marketing/onboarding body text kept under ~65 characters.

---

## 3. Spacing

8px base unit. These match Tailwind's default spacing scale exactly (`0.25rem` = 4px per step), so no custom config is needed — the "Tailwind class" column below uses stock utilities.

| Token | Value | Tailwind class (e.g. padding) |
|---|---|---|
| `space-1` | 4px | `p-1` |
| `space-2` | 8px | `p-2` |
| `space-3` | 12px | `p-3` |
| `space-4` | 16px | `p-4` |
| `space-5` | 24px | `p-6` *(Tailwind's own step 6, not 5, lands on 24px)* |
| `space-6` | 32px | `p-8` |
| `space-8` | 48px | `p-12` |
| `space-10` | 64px | `p-16` |

Applied conventions seen in code:
- Card internal padding: `space-4` (compact cards) to `space-5`–`space-6` (panels).
- Gap between stacked link cards: `space-3` (`gap-3`, confirmed in `LinkCard`/`DashboardScreen`).
- Section vertical padding on the landing page: `py-14`/`py-16` mobile → `py-20`/`py-24` desktop (i.e. roughly `space-6`→`space-8` scaling up at `sm:`).

---

## 4. Radius

Added to `tailwind.config.js` under `theme.extend.borderRadius` (overrides the matching default keys).

| Token | Value | Tailwind class | Usage |
|---|---|---|---|
| `radius-sm` | 8px | `rounded-sm` | Small icon buttons |
| `radius-md` | 12px | `rounded-md` | Inputs, secondary buttons |
| `radius-lg` | 16px | `rounded-lg` | Link cards, primary buttons |
| `radius-xl` | 24px | `rounded-xl` | Feature cards (marketing) |
| `radius-2xl` | 32px | `rounded-2xl` | Public-profile card, phone-preview frame |
| `radius-pill` | 999px | `rounded-pill` | Primary CTA buttons, tags, URL bar, toggle track |

```js
// tailwind.config.js — theme.extend.borderRadius
borderRadius: {
  sm: "8px",
  md: "12px",
  lg: "16px",
  xl: "24px",
  "2xl": "32px",
  pill: "999px",
}
```

---

## 5. Shadow / Elevation

| Token | Tailwind class | CSS value | Usage |
|---|---|---|---|
| `shadow-card` | `shadow-card` | `0 1px 2px rgba(0,0,0,0.04), 0 2px 8px rgba(0,0,0,0.06)` | Resting elevation: link cards, panels |
| `shadow-floating` | `shadow-floating` | `0 4px 16px rgba(0,0,0,0.10)` | Overlaid elements: floating icon buttons, badges |

```js
// tailwind.config.js — theme.extend.boxShadow
boxShadow: {
  card: "0 1px 2px rgba(0,0,0,0.04), 0 2px 8px rgba(0,0,0,0.06)",
  floating: "0 4px 16px rgba(0,0,0,0.10)",
}
```

Note: a couple of built components use ad hoc raw shadows instead of these tokens (e.g. the public-profile share-copy tooltip and join-CTA bar use `shadow-md`/`shadow-lg` stock Tailwind utilities). Worth normalizing to `shadow-card`/`shadow-floating` for consistency — flagged here rather than silently "fixed" in this doc.

---

## 6. Breakpoints

No custom breakpoints were defined — every screen built so far uses Tailwind's stock breakpoint scale directly (`sm:`, `md:`, `lg:` prefixes appear throughout the onboarding, dashboard, and public-profile code).

| Breakpoint | Min width | Observed usage in built screens |
|---|---|---|
| *(default)* | 0px | Mobile: single column everywhere |
| `sm` | 640px | Minor spacing/type bumps (e.g. landing page section padding) |
| `md` | 768px | Dashboard: icon rail switches vertical↔horizontal, preview column appears; onboarding/preview toggle threshold |
| `lg` | 1024px | Onboarding split-screen visual panel appears; dashboard preview column widens; public-profile QR corner appears |
| `xl` | 1280px | **TBD** — not yet used by any built screen |
| `2xl` | 1536px | **TBD** — not yet used by any built screen |

---

## 7. Motion

No formal motion system (easing curves, duration tokens, choreography rules) has been defined. What follows is an inventory of what's actually used in code today — treat everything not listed here as **TBD**.

| Interaction | Value used | Where |
|---|---|---|
| Button hover (color) | `transition-colors duration-150` | `PillButton` |
| Link-card hover (shadow) | `transition-shadow` (Tailwind default duration, 150ms) | Public-profile link pills |
| Accordion chevron rotate | `transform 150ms ease` | FAQ `FaqItem` (landing page) |
| Icon-rail active state | `transition-colors` (no explicit duration set → Tailwind default) | `IconRail` |

**TBD:**
- Named duration/easing tokens (e.g. `--motion-fast`, `--motion-ease-out`) — not defined; the `150ms` above is repeated as a literal in each component rather than referencing a shared value.
- Page transition / route-change animation — none implemented.
- `prefers-reduced-motion` handling — not implemented anywhere.
- Any orchestrated entrance animation (e.g. hero load-in) — none built; all current motion is hover/interaction-triggered only, which is consistent with the "motion sparingly, answer a person's action" principle in `design.md`, but it hasn't been made a documented rule anywhere.
