# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start dev server at http://localhost:5173
npm run build    # Production build (outputs to dist/)
npm run preview  # Serve the production build locally
npm run lint     # Run ESLint
```

There are no tests in this project.

## Architecture

Single-file React app — all state and UI live in `src/App.jsx`. There is no router, no context, no external state library, and no component decomposition yet; everything is one component.

**State shape** (`App.jsx`):
- `transactions` — array of `{ id, description, amount, type, category, date }`. `amount` is stored as a **string** (not a number), which causes the summary totals to concatenate instead of add. This is a known bug.
- `description`, `amount`, `type`, `category` — controlled form fields for the add-transaction form.
- `filterType`, `filterCategory` — filter selectors for the transaction list.

**Data flow**: transactions are filtered inline (no derived state abstraction) and rendered directly in the return. There is no persistence — state resets on page reload.

**Styling**: plain CSS in `src/App.css` (component styles) and `src/index.css` (global reset). No CSS framework or CSS modules. The `.delete-btn` class is defined in `App.css` but not yet wired to any button in the JSX.

**Known bugs**:
- `amount` stored as string → `reduce` sum concatenates strings. Fix: parse with `parseFloat(t.amount)` in the reduce callbacks.
- Transaction seed data: "Freelance Work" (id 4) is typed `"expense"` but categorized `"salary"` — likely a copy-paste error.
