---
noteId: "b1208d20a7d111f1bcf2d70766d52b5b"
tags: []

---

# mylinkit — UI Components & Patterns

Companion to `design-system.md` (tokens). This doc inventories every component across the built screens (onboarding, dashboard, public profile, landing), plus components the design system specced but that were never coded — those are marked **Not yet built**.

---

## 1. Components

### 1.1 PillButton

`components/ui/PillButton.tsx`

| Variant | Fill | Text | Usage |
|---|---|---|---|
| `primary` | `bg-accent`, hover `bg-accent-hover` | `text-text-primary` | Marketing/product CTAs |
| `secondary` | `bg-bg-subtle`, hover `bg-border` | `text-text-primary` | Lower-emphasis actions |
| `dark` | `bg-text-primary` | `text-text-inverse` | Full-width primary action (e.g. onboarding "Continue") |

**States:** default, hover (variant-dependent background shift), `disabled` (opacity 50%, `cursor-not-allowed` — used when a handle isn't yet available or a form field is empty).
**Props:** `variant`, `fullWidth`, plus all native `<button>` attributes.

---

### 1.2 Input field

No single shared `<Input>` component exists yet — the pattern is repeated inline in three places: the onboarding handle field, the landing-page email field, and the add/edit link-title fields. Same visual spec throughout:

| State | Style |
|---|---|
| Idle | `bg-bg-subtle`, no visible border |
| Focus | 1px accent-colored ring (email/link inputs use browser-default `outline`; the onboarding handle field uses a manual `box-shadow` ring keyed to validation state — see below) |
| Success (onboarding handle: available) | `box-shadow: 0 0 0 1.5px var(--color-accent)` + green check icon + green message text |
| Error (onboarding handle: taken / too short) | `box-shadow: 0 0 0 1.5px #E0455B` + red X icon + red message text |
| Loading (onboarding handle: checking) | Spinner icon (`Loader2`, `animate-spin`), no ring color change |

**Not yet built:** a shared `<Input>` component — recommend extracting one before adding a fourth use site, and formalizing the focus ring as a token rather than the current mix of manual `box-shadow` and default `outline`.

---

### 1.3 Link Card — two variants

**Editor variant** — `components/dashboard/LinkCard.tsx`
- View state: drag handle (`GripVertical`), thumbnail placeholder square, title (bold, truncated) + meta line (`{clicks} clicks · {url}`), visibility toggle (`Eye`/`EyeOff` icon button — see 1.7 note on the toggle-switch mismatch), overflow menu (`⋮` → Edit / Delete).
- Editing state: view swaps for two inline text inputs (title, URL) + Cancel/Save buttons.
- Hidden state: whole card drops to 50% opacity (`link.visible === false`) but stays visible and editable in the dashboard list — it's just excluded from the live preview and public page.
- Draggable via native HTML5 drag-and-drop (`draggable`, `onDragStart`/`onDragOver`/`onDrop`) — reordering triggers `POST /api/links/reorder`.

**Public variant** — inline in `components/profile/PublicProfileScreen.tsx` (not extracted as a shared component)
- Leading icon (mapped from a `ProfileLinkIcon` key — `udemy`, `youtube`, `newsletter`, `portfolio`, `twitter`, `instagram`) + label, left-aligned.
- Hover: `shadow-sm` → `shadow-md`.
- No drag handle, no overflow menu, no visibility toggle — anonymous visitors only ever see the finished list.
- **Deviation from `design.md` 6.4:** the original spec called for centered text with no icon on the public variant. Icons were added when the profile grew to six links, since six unlabeled centered pills were harder to scan than two. Revert if strict spec-parity matters more than scannability.

---

### 1.4 AddLinkForm

`components/dashboard/AddLinkForm.tsx`
- Single state: two inputs (title, URL) + Cancel / "Add link" buttons.
- Validation: title is required (empty submit is a no-op); URL falls back to a placeholder domain if left blank — **TBD** whether that fallback is actually desirable UX or should instead be a required field with inline error text.

---

### 1.5 IconRail

`components/dashboard/IconRail.tsx`
- Items: Content, Header, Design, Settings (icons only, `LayoutList` / `ImageIcon` / `Paintbrush` / `Settings`).
- Orientations: `vertical` (desktop, icon + label caption) and `horizontal` (tablet/mobile, icon only, no caption).
- States: active (black rounded-square background, inverted icon color) / inactive (transparent, secondary icon color).
- **Not yet built:** Header, Design, and Settings panels themselves — only "Content" renders anything; selecting the other three currently does nothing.

---

### 1.6 Phone Preview — two variants

**Live variant** — `components/dashboard/PhonePreview.tsx`
- Reads actual dashboard state; renders only `visible: true` links; shows an empty state ("No visible links yet") when the list is empty or everything's hidden.
- Sticky-positioned within the dashboard preview column at `md:` and up.

**Decorative variant** — `components/onboarding/HandlePreviewPanel.tsx`
- Static illustrative collage (two layered mock profile cards with placeholder content), not driven by real data.
- Includes a floating badge that echoes the handle being typed (`/{handle}`), the one piece of it that is live.
- Hidden below `lg`.

---

### 1.7 Toggle switch — **Not yet built** (spec/implementation mismatch)

`design.md` section 6.5 specs a pill-track toggle switch (green when on) for link visibility. What actually shipped in `LinkCard` is an `Eye`/`EyeOff` icon button instead — functionally equivalent (click to show/hide) but visually different from the spec, and it doesn't reuse the same on/off treatment as, say, a future "public profile visible" account-level setting might need. Reconcile before building any other on/off control, so there's one visual language for binary state instead of two.

---

### 1.8 Tabs

Implemented inline (not extracted as a shared component) in two places:
- `DashboardScreen.tsx` — "Links" / "Shop" tabs, underline style (2px bottom border on active, secondary-color text on inactive).
- Landing page has no tabs.

**Not yet built:** a reusable `<Tabs>` component — if a third tab set shows up anywhere, extract one rather than copying the inline pattern again.

---

### 1.9 Mobile Edit/Preview toggle

Dashboard-only pattern, inline in `DashboardScreen.tsx`. Two-button segmented control ("Edit" / "Preview") shown only below `md`, toggling which of the two columns is visible. This is the mechanism that keeps the live preview reachable on mobile without hiding it behind a separate route or modal.

---

### 1.10 Floating circular icon button

Recurring pattern, not extracted into a shared component — appears in three places with identical styling (`w-9 h-9`/`w-11 h-11` circle, `bg-bg`, `shadow-floating`-equivalent):
- Dashboard preview column: sliders icon, external-link icon.
- Public profile: brand badge (top-left), share/copy icon (top-right).
- Onboarding decorative panel: floating handle badge (pill-shaped, not circular — visually related but not the same component).

**Not yet built:** a shared `<FloatingIconButton>` — currently three copy-pasted instances.

---

### 1.11 Accordion (FAQ)

`FaqItem`, built inline in the landing-page component (not in a shared `components/ui` location).
- States: open / closed, single-open-at-a-time (`openIndex` state, not a multi-open accordion).
- Chevron rotates 180° on open (`transform 150ms ease`).
- Bottom hairline divider (`border-border`) between rows; no background change on the row itself.

**Not yet built:** a reusable `<Accordion>` — if this pattern is needed anywhere outside the marketing FAQ (e.g. a help-center page later), extract it.

---

### 1.12 SignupForm

Landing page only (hero + final CTA), two instances of the same component with a `dark` prop for the final-CTA variant.
- States: `idle`, `loading` ("Sending..." button label), `success` (form replaced by a confirmation line), `error` (state exists in code but **no visible error message is rendered** — currently indistinguishable from `idle` if the mocked submit rejects). **TBD:** add visible error copy before wiring to a real endpoint.
- Submission is currently mocked (`setTimeout`, no real network call) — see inline comment in the component for exactly where to swap in a real `fetch("/api/subscribe", ...)`.

---

### 1.13 Join CTA bar

Public profile only, fixed to the viewport bottom over a gradient scrim. Dismissible (`X` button → `ctaDismissed` state, no persistence — reappears on reload). **TBD:** whether dismissal should persist (cookie/localStorage) so a visitor who dismisses it doesn't see it again on the same session.

---

### 1.14 QR code block

Public profile, desktop only (`lg:` and up). Currently a **decorative placeholder** — a deterministically-generated grid pattern that looks QR-like but encodes nothing. **Not yet built:** a real QR generator (e.g. the `qrcode` package rendered server-side to SVG/data URL) needs to replace this before the "View on mobile" affordance actually works.

---

### 1.15 Unclaimed-handle screen

`components/profile/UnclaimedHandle.tsx` — shown when `app/[handle]` doesn't match a known profile. Single state: headline, one line of body copy, "Claim this handle" button linking to `/onboarding?handle={handle}`.

---

### 1.16 Announcement / promo bar — intentionally not built

`design.md` specs a top black announcement bar with an "Upgrade" CTA (section 6.9), visible in the original dashboard reference screenshot. This was explicitly dropped from the dashboard per product decision (own product, no self-upselling). It still appears in the marketing-landing layout spec (`design.md` 7.3, the lime promo strip above the nav) as an illustrative section only — not implemented as a reusable component anywhere.

---

## 2. Layout Patterns

| Screen | Route | Breakpoint behavior |
|---|---|---|
| Onboarding | `app/onboarding/page.tsx` | `< lg`: single column, form only, centered, max-width. `lg+`: 50/50 grid, form left / decorative visual panel right. |
| Dashboard | `app/dashboard/page.tsx` | `< md`: single column, icon rail becomes a horizontal top strip, Edit/Preview segmented toggle controls which panel shows. `md`: icon rail (horizontal strip retained) + editor + preview side by side. `lg+`: icon rail switches to a vertical labeled sidebar; preview column widens (`360px` → `400px`) and becomes sticky. |
| Public profile | `app/[handle]/page.tsx` | All sizes: centered card, `max-w-[420px]`, fluid down to viewport width with `px-4` gutter. QR corner block only appears `lg+`. Join-CTA bar is full-width and present at every size. |
| Landing | (page not yet routed — currently only exists as an artifact preview) | `< sm`: all sections single-column, stacked. `sm+`: three-reasons strip becomes a 3-column grid; hero/CTA form rows go from stacked (`flex-col`) to inline (`sm:flex-row`). |

General rules observed across all four:
- Mobile-first: unprefixed classes are the small-screen baseline; `sm:`/`md:`/`lg:` add complexity, never remove it.
- No screen uses `xl:` or `2xl:` — content max-widths (`max-w-2xl`, `max-w-[420px]`, etc.) cap growth instead of adding new breakpoint-specific layouts past `lg`.

---

## 3. Accessibility Notes

Documenting actual current state, not aspirational state — several gaps below are real and should be fixed, not assumed handled.

**What's in place:**
- A few icon-only buttons have `aria-label`s: the onboarding handle input (`aria-label="Choose your handle"`), the public-profile share button (`aria-label="Copy profile link"`), and its dismiss button (`aria-label="Dismiss"`).
- The onboarding handle-availability status is never color-only — every state (`checking`/`available`/`taken`/`invalid`) pairs an icon *and* a text message, not just a ring color.
- Semantic `<a href>` tags are used for the public-profile links (not `<div onClick>`), so they're keyboard-reachable and correctly announced as links by default.

**Gaps — TBD / not yet addressed:**
- **Most icon-only buttons have no `aria-label`:** the drag handle, overflow-menu (`⋮`) trigger, visibility eye-toggle, and the floating sliders/external-link buttons in the dashboard preview column are all unlabeled. A screen reader currently announces these as generic unlabeled buttons.
- **Drag-and-drop reorder has no keyboard equivalent.** The native HTML5 drag implementation is mouse/touch-only; there's no "move up" / "move down" fallback for keyboard or screen-reader users. This should be added before shipping the reorder feature.
- **No custom focus-visible styling anywhere.** Every interactive element relies on the browser's default focus ring (or, in a couple of cases — the pill inputs — no visible focus indicator at all beyond the box-shadow validation ring, which only appears once text is entered). A consistent `focus-visible:` treatment is TBD.
- **Color contrast has not been audited.** `text-secondary` (`#6B6B70`) on `bg-subtle` (`#F5F5F7`) in particular is worth checking against WCAG AA before shipping — it's used for meta text and helper copy throughout.
- **`prefers-reduced-motion` is not handled** anywhere (see `design-system.md` §7).
- **No alt text strategy for avatars/images**, because none of the built screens use real `<img>` tags yet — all avatars are solid-color placeholder `<div>`s. Once real photo uploads exist, decide the alt-text convention (e.g. `"{handle}'s profile photo"`) before wiring it in.
- **Landmark/semantic structure not verified.** Pages are built with generic `<div>` wrappers; whether `<main>`, `<nav>`, `<header>` landmarks are present and correctly nested hasn't been checked.
- **Modal-like surfaces (the link-card overflow menu) don't trap focus or close on Escape.** It's a simple conditionally-rendered `<div>`, not a proper popover/menu with keyboard handling.
