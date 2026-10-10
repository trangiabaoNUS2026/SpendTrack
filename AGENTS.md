# SpendTrack — AI Development Instructions
Read `docs/PRODUCT.md`, `docs/TECHNICAL.md`, `docs/ROADMAP.md`, `docs/DECISIONS.md`, and `docs/TEST_CASES.md` before implementing changes.

You are assisting a beginner with basic Python knowledge. Explain concepts clearly and prefer simple, readable code.

1. Build React + TypeScript + Vite, Dexie/IndexedDB, offline-first PWA. No backend, authentication, paid APIs or sync in P1.
2. Implement only the requested roadmap phase. Inspect existing files and propose a short plan before editing.
3. Use integer cents, correct local date handling, exact reconciliation, immutable completed-week configurations and recomputable balances.
4. Separate UI, pure business calculations, and database operations.
5. Preserve existing user records and migration compatibility; never reset data as a shortcut.
6. Use atomic Dexie transactions for related writes. Validate external receipts and backups before mutation.
7. Add automated tests for calculations, schema changes, imports and restore safety.
8. Run formatting, linting, typechecking, tests and production build; report only commands actually run.
9. Avoid unrelated features, unnecessary packages, and speculative refactors.
10. At end report files changed, tests/results, manual verification steps, limitations and next task; update roadmap status.
