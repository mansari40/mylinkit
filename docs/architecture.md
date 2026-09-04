---
noteId: "b1208d20a7d111f1bcf2d70766d52b5b"
tags: []

---

# mylinkit — Architecture

App structure, routing strategy, and code organization conventions.

---

## 1. Routing

The app uses **Next.js App Router** with the following default conventions:

- **Server Components** are the default. Every component is a Server Component unless explicitly marked with `"use client"`.
- **Client Components** are used only when interactivity is required (event handlers, state, effects, browser APIs).
- Pages live in `app/` and follow file-system routing.

---

## 2. Code Organization

### `/lib` — Shared logic

Shared utilities, helpers, and configuration that are not UI. Each concern gets its own subfolder:

| Folder | Purpose |
|---|---|
| `lib/db` | Database client, connection setup, query helpers |
| `lib/auth` | Authentication helpers, session handling, middleware |
| `lib/types` | Shared TypeScript types, interfaces, and enums |

Other `/lib` modules (e.g. validation schemas, email, analytics) are added as needed, each in their own file or subfolder.

### `/components` — UI

| Folder | Purpose |
|---|---|
| `components/ui` | Generic, reusable UI primitives (buttons, inputs, modals, etc.) — no business logic |
| `components/[feature]` | Feature-specific components (e.g. `components/dashboard`, `components/profile`) — contain business logic and compose primitives from `ui/` |

---

## 3. Naming Conventions

| Element | Convention | Example |
|---|---|---|
| Files | `kebab-case` | `link-card.tsx`, `use-profile.ts` |
| Components | `PascalCase` | `LinkCard`, `PhonePreview` |
| Server Actions | `verb-first` | `createLink`, `updateProfile`, `deleteLink` |
| Utility files | `kebab-case` | `format-date.ts`, `validate-handle.ts` |

---

## 4. Server Actions

Server Actions are the primary mutation pattern. They follow these conventions:

- Named with a **verb-first** convention: `createLink`, `updateProfile`, `deleteLink`.
- Defined in the file closest to where they are used (colocated), or in a shared `/lib` module if reused across features.
- Always handle errors and return structured responses.
