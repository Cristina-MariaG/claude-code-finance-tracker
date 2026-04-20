# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

All commands run from the project root (`expense-tracker-starter/`):

- `npm install` — install dependencies
- `npm run dev` — start Vite dev server at http://localhost:5173
- `npm run build` — production build to `dist/`
- `npm run preview` — serve the production build locally
- `npm run lint` — run ESLint over `**/*.{js,jsx}`

There is no test runner configured.

## Architecture

This is a **single-component React app** built with Vite (React 19, ESM, `type: "module"`). Entry point is `src/main.jsx` → `src/App.jsx`. All state, business logic, form handling, filtering, and rendering live in `App.jsx` — there are no sub-components, no router, no API, and no persistence layer. Data is seeded in-memory and lost on reload.

Lint config (`eslint.config.js`) uses the flat-config format with `@eslint/js` recommended, `eslint-plugin-react-hooks`, and `eslint-plugin-react-refresh` (Vite variant). `no-unused-vars` ignores identifiers starting with an uppercase letter or underscore.

## Context from README

Per the README, this is a **course starter project** that "intentionally has a bug, poor UI, and messy code." Expect to find issues rather than polish — for example, `amount` is stored as a string on each transaction, so the `reduce` calls in `App.jsx` concatenate strings instead of summing numbers. Treat pre-existing "smells" as likely intentional teaching material; confirm with the user before aggressive rewrites.
