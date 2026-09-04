# AGENTS.md

## Project Overview

Next.js 16 application using TypeScript, Tailwind CSS v4, and App Router.

## Commands

- `npm run dev` - Start development server
- `npm run build` - Production build
- `npm run lint` - Run ESLint
- `npm start` - Start production server

## Structure

- `src/app/` - App Router pages and layouts
- Import alias: `@/*` maps to `src/*`

## Conventions

- TypeScript strict mode enabled
- ESLint with next/core-web-vitals and typescript configs
- Tailwind CSS v4 with PostCSS plugin

## Doc Convention

Whenever a new file is created in `docs/`, add it to the **Project Docs** table below with one line describing what it covers and when to read it.

### Project Docs

| File | Covers | When to read |
|---|---|---|
| `docs/design-system.md` | Design tokens — colors, typography, spacing, radius, shadows, breakpoints, motion. | Before adding or modifying any Tailwind classes, tokens, or theme config. |
| `docs/ui.md` | Component inventory — built components, layout patterns across screens, and accessibility notes. | Before creating a new component, changing an existing one, or working on a11y fixes. |
| `docs/architecture.md` | App structure — routing strategy, code organization, naming conventions, and server actions. | Before adding a new route, creating files or folders, or writing Server Actions. |
| `docs/database.md` | MongoDB with Mongoose — connection pooling, schema conventions, indexes, and query rules. | Before adding or modifying a model, writing a query, or changing the DB connection setup. |
