# SpendTrack — Development Roadmap
Status: no phases started. Complete each phase, test it, verify on Android where relevant, then commit before proceeding.

| Phase | Deliverable | Exit gate |
|---|---|---|
| 0 | React/TS/Vite/Tailwind shell, three tabs, PWA, deployment | Build, offline shell, Android installation |
| 1 | Dexie schema, categories, quick entry, CRUD | Data persists; money validation; tests |
| 2 | JSON backup, validation, atomic restore, reminder | Fresh-profile round trip; rollback on invalid input |
| 3 | Budget engine, current-week settings, historical configs, category caps, dashboard | Week and rollover tests pass |
| 4 | Optional itemization, mixed categories, adjustments | Exact reconciliation; no double counting |
| 5 | Linked full/partial refunds | Date, caps, per-item refund invariants tested |
| 6 | External-AI prompt, JSON/CSV import, editable review | Invalid data blocked; atomic save |
| 7 | Search/filter and CSV exports | Item-name search; correct category filters |
| 8 | Release validation, migration/offline/restore testing | No known critical financial/data-loss defects |

## Per-phase workflow
Read AGENTS.md and docs. Inspect files. Plan changes. Implement only current phase. Add tests. Run formatting, lint, typecheck, tests, build. Manually verify. Update ROADMAP.md and DECISIONS.md if needed. Commit to Git.

## Future only
P2 category suggestions, merchant recognition, tags, forecasts. P3 analytics, recurring expenses, price history. Optional cloud sync or Capacitor packaging only after reliable P1.
