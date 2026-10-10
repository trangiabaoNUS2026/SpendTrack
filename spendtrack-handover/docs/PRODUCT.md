# SpendTrack — Product Requirements Document
Version: 1.0 (frozen P1 scope) | 2026-10-10

## Vision
Android-first, installable, offline-first personal expense tracker answering: how much remains this week, where money went, and what was bought. Fast quick entry with optional itemization. SGD only. No accounts, backend, cloud sync, tracking, or paid AI API.

## Navigation and UX
Three tabs: Dashboard, History, Settings; floating Add Expense action opens quick form. Dashboard prioritizes signed remaining budget, weekly date range, net counted spending, base budget, optional rollover, category caps/progress, recent transactions. No per-day allowance. Clean mobile-first touch UI, accessible labels, clear validation.

## Weekly budgets
Weeks run Saturday–Friday using Asia/Singapore local calendar dates. Purchase date (not entry timestamp) determines week. Both fixed and positive-only rollover modes supported. Current-week changes to base budget or mode take effect immediately; completed weeks retain their effective configuration. Rollover incoming = max(0, prior week's remaining) when current mode is rollover; otherwise zero. Overspending displays negative remaining, but negative carryover is never propagated. Editing past transactions recalculates affected later balances without rewriting historical configuration. Initial onboarding week has no prior carryover. Do not display daily allowance.

## Categories
Default categories: Groceries, Eating Out, Transport, Entertainment, Shopping, Education, Household, Other. Users can add, rename, archive, and unarchive. Referenced categories cannot be hard deleted. Optional weekly category caps, with effective-week history. Excluded transactions do not affect weekly budget or capped-category spending. Caps do not alter overall budget.

## Transactions
Quick form requires positive amount, description, category; date defaults to today; countsTowardBudget defaults true. Merchant, payment method, notes, and items optional. Create/view/edit/delete (with confirmation). Refunds are linked negative transactions on the date received; partial refunds permitted and cannot exceed original refundable amounts. Original purchases with linked refunds cannot be deleted without resolving them. Transfers and income tracking are out of scope.

## Itemization
Zero or more items per transaction. If none, transaction category determines category spending. If items exist, item categories exclusively determine category spending; never count both. Each line stores name, quantity, unit, unit price, actual signed line total, category, line type (product/fee/tax/discount/adjustment). Actual line total is authoritative. Discounts/fees/taxes are explicit signed lines. Item lines must sum exactly to transaction amount before saving. Itemized refunds reverse original item allocations, without exceeding refundable line amounts.

## Receipt-assisted entry
Generate a copyable prompt containing active category names for external AI; accept pasted JSON or CSV. Normalize, validate, show editable review, flag unknown categories and missing values, block saving until required fields and exact totals reconcile. Never auto-create categories. Save transaction and lines atomically. No built-in AI API.

## History
Reverse-chronological history, edit/delete/details, filters for date, merchant, category, amount, inclusion; case-insensitive search of description and item names. Category filtering respects item allocation.

## Data ownership
All records stored locally in IndexedDB; no server. Installable PWA must work offline after initial caching. Manual versioned JSON backup and validated replace-all restore with confirmation; CSV export for analysis. Remind user about backup age; browser storage may be lost and persistence requests do not guarantee retention. No automatic cloud backup in P1.

## P2 / P3 deferred
P2: category suggestions, merchant recognition, tags, better payment metadata, forecasts, advanced budgeting. P3: reports, trends, recurring expenses, merchant comparisons, grocery composition, frequent items, unit-price history. Optional future sync and Capacitor packaging may be considered but must not complicate P1.

## P1 completion
Core flows, rollover, category caps, mixed-category items, refunds, receipt review, search, backup/restore, CSV export, and offline installation work on a physical Android phone. Critical financial and data-integrity tests pass. No known data-loss or accounting defects.
