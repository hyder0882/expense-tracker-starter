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

React + Vite app with no router, no context, and no external state library.

**Component tree:**
```
App
├── Summary          — computes and displays income/expense/balance totals
├── TransactionForm  — owns form state, calls onAdd(transaction) prop
└── TransactionList  — owns filter state, renders the filtered table
```

**State ownership:**
- `App` — holds the `transactions` array (`{ id, description, amount, type, category, date }`). `amount` is always a number. Seed data lives in the module-level `initialTransactions` constant. Passes `transactions` down and provides `handleAdd` to `TransactionForm`.
- `TransactionForm` — owns `description`, `amount`, `type`, `category` locally. Builds the transaction object and calls `onAdd`; resets its own fields after submission.
- `TransactionList` — owns `filterType` and `filterCategory` locally. Filters the received `transactions` array inline before rendering.
- `Summary` — stateless; derives `totalIncome`, `totalExpenses`, and `balance` from the `transactions` prop.

Both `TransactionForm` and `TransactionList` define their own local `categories` constant — it is a static list, not shared state.

**Styling**: plain CSS in `src/App.css` (component styles) and `src/index.css` (global reset). No CSS framework or CSS modules. The `.delete-btn` class is defined in `App.css` but not yet wired to any button in the JSX.

**No persistence** — state resets on page reload.

**Known issue**: seed transaction "Freelance Work" (id 4) is typed `"expense"` but categorized `"salary"` — likely a copy-paste error.
