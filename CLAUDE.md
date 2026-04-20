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

A React 19 + Vite SPA (ESM, `type: "module"`). Entry point: `src/main.jsx` → `src/App.jsx`. No router, no API, no persistence — seed data lives in `App.jsx` state and is lost on reload.

**Component layout (`src/`):**

- **`App.jsx`** — orchestrator. Owns the single source of truth: the `transactions` state and `handleAdd`. Also defines the module-level `categories` constant passed down as a prop. Renders `Summary`, `TransactionForm`, `TransactionList`.
- **`Summary.jsx`** — receives `transactions`, computes `totalIncome` / `totalExpenses` / `balance` internally via `filter` + `reduce`, renders the three summary cards.
- **`TransactionForm.jsx`** — owns its own input state (`description`, `amount`, `type`, `category`). On submit, builds the transaction object (coercing `amount` via `Number()`) and invokes the `onAdd` callback prop, then resets its inputs.
- **`TransactionList.jsx`** — owns its own filter state (`filterType`, `filterCategory`). Receives `transactions` + `categories`, filters locally, renders the table.

**Data-flow invariants worth knowing:**

- `transaction.amount` is a **number**, not a string — seed data uses numeric literals, and `TransactionForm` coerces the input value via `Number(amount)` before calling `onAdd`. Do not reintroduce string amounts; the `Summary` reducers rely on numeric addition.
- `categories` is a plain module-level constant in `App.jsx`, passed explicitly as a prop to children rather than imported. If adding a new consumer, keep the single source of truth.

**Lint** (`eslint.config.js`): flat config with `@eslint/js` recommended + `eslint-plugin-react-hooks` + `eslint-plugin-react-refresh` (Vite variant). `no-unused-vars` ignores identifiers starting with an uppercase letter or underscore.

## Context from README

This codebase started from a public course starter described as intentionally containing "a bug, poor UI, and messy code." The initial string-amount bug has been fixed and the single `App.jsx` has been split into the components above. Further UI/UX polish and refactors are ongoing learning work — when in doubt about whether a rough edge is intentional course material vs. something to clean up, confirm with the user before broad rewrites.
