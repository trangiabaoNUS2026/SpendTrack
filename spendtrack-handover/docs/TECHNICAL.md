# SpendTrack — Technical Specification
Version 1.0 | React + TypeScript + Vite + Tailwind + Dexie/IndexedDB + vite-plugin-pwa + Vitest + fake-indexeddb + Playwright. Cloudflare Pages static hosting. No backend.

## Domain model (logical; implement Dexie indexes in Phase 1)
- transactions: id UUID, kind ('expense'|'refund'), date YYYY-MM-DD, description, merchant?, categoryId, amountCents signed integer, countsTowardBudget boolean, paymentMethod?, notes?, originalTransactionId? (refund only), createdAt ISO timestamp, updatedAt ISO timestamp.
- items: id UUID, transactionId, originalItemId? (refund allocation), name, quantity decimal string, unit, unitPriceCents signed integer or null, lineTotalCents signed integer, categoryId, lineType ('product'|'fee'|'tax'|'discount'|'adjustment').
- categories: id UUID, name, archivedAt?, createdAt.
- weeklyBudgets: weekStart YYYY-MM-DD (Saturday, primary key), baseBudgetCents nonnegative integer, rolloverEnabled boolean. A record is created for onboarding week, and for weeks in which settings change. Missing weeks inherit most recent preceding configuration; when a week completes its effective configuration must remain reproducible. A setting change never changes an earlier week's configuration.
- categoryCaps: id, categoryId, effectiveWeek YYYY-MM-DD (Saturday), capCents nonnegative integer or null. Effective-week configuration inherited until changed; past effective caps preserved.
- settings: key/value for nonfinancial preferences and initialization date. Keep current financial settings in effective-week records, not contradictory copies.
- metadata: key/value for app version, backup export attempt metadata and similar local information.

All money integer cents; user-facing SGD decimal strings parsed with exact decimal rules. Do not use floating-point for financial totals. Quantities stored as decimal strings; use decimal-safe arithmetic if deriving prices. Unit price is informational; actual line total is authoritative.

## Date and budget engine
Purchase date is local calendar date, not UTC timestamp. Determine 'today' in Asia/Singapore. Week start is preceding Saturday inclusive. Avoid adding 86,400,000 milliseconds to dates.

For each week w:
  countedNet(w) = sum(amountCents for transactions dated in w with countsTowardBudget=true)
  carryIn(w) = rolloverEnabled(w) ? max(0, remaining(w-1)) : 0
  available(w) = baseBudgetCents(w) + carryIn(w)
  remaining(w) = available(w) - countedNet(w)
Initial onboarding week carryIn=0. A week with zero transactions still participates. Fixed-mode weeks can leave positive remaining that the following rollover-mode week carries in. Negative spending from refunds is permitted. Recompute downstream balances when historical spending changes; do not persist computed balances.

For category c and week w, sum allocations only for budget-included transactions. Non-itemized transaction allocation = its amount to transaction.categoryId; itemized allocation = signed item line totals grouped by item.categoryId. Refunds follow same allocation rule with negative amounts. Caps are warnings, never sub-budgets deducted from overall allowance. Preserve cap effective history.

## Refund invariants
Refund transaction amount negative, originalTransactionId required, original purchase must exist. Total absolute refunds cannot exceed original transaction total; per-item linked refund lines cannot exceed original item refundable balance. For itemized mixed-category originals, refunds must use line allocations referencing originalItemId. Refund inclusion defaults from original at creation, then is independently stored. Reject edits to originals that invalidate existing refunds. Refund date is date received. Deleting referenced originals is blocked.

## Data integrity and transactions
- Itemized transaction: sum(items.lineTotalCents) === transaction.amountCents exactly.
- Ordinary expenses positive; refunds negative. All references valid.
- Category archived status does not invalidate historical references.
- Use Dexie transactions to atomically save parent and children, deletes, imports, and restore.
- Database migrations must preserve existing records; never delete user data to fix schema.
- IDs are UUIDs, suitable for future sync, but sync is not implemented.

## Backup
Versioned JSON object with schemaVersion:1, exportedAt, all domain tables and settings, and record counts. Validate entire input including references, dates, integer cents, refund constraints, reconciliation, and duplicate IDs before mutation. Reject unsupported schema versions. Show preview and confirm replacement. Perform replace-all in a single Dexie transaction across all affected tables. Roll back on failure. CSV export separately outputs transactions and items with parent IDs; CSV is not a restoration format. Backup timestamp means generated/download initiated, not guaranteed safe external storage. Verify restore in fresh browser profile.

## Receipt format
Canonical JSON version 1 uses schemaVersion, merchant, date, total decimal string, optional description, and items containing name, quantity decimal string, unit, unitPrice decimal string or null, lineTotal decimal string, category name, lineType. CSV must define a stable header mapping and one row per item, with repeated transaction-level fields; document exact headers during Phase 6. Both parse to one intermediate typed representation. No direct execution of external text; validate and review before atomic save. Unknown categories require assignment.

## PWA/offline/security
Cache app shell and assets with vite-plugin-pwa; do not cache sensitive financial records in service worker caches. IndexedDB holds records; request persistent storage when available but warn it is not guaranteed. Offline all tracking and backup/restore after install. No analytics, advertising, or server transmission of records. Backup unencrypted in P1: user should store securely. Avoid sensitive logs.

## Testing
Vitest for pure calculations, parsing and validation; fake-indexeddb for Dexie integration; Playwright for critical UI flows, PWA/offline behavior where practical. Run typecheck, lint, unit/integration tests and production build each phase. Android manual tests at milestones.
