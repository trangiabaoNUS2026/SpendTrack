# SpendTrack — Decision Log
2026-10-10 — Frozen P1 decisions. Changes require dated amendments and test updates.

- Android-first installable PWA; React/TypeScript/Vite; local-first IndexedDB/Dexie; static hosting, no backend or accounts.
- SGD cents; Saturday–Friday Singapore-local weeks; purchase date controls spending week.
- Both fixed and positive-only rollover; overspending shows signed negative remaining and does not carry as debt.
- Current-week budget amount and mode changes apply immediately. Completed weeks' configurations preserved; calculated balances can change when historical spending is edited.
- No daily allowance display.
- Category caps optional, weekly, historical cap values preserved. Excluded transactions do not count toward either budget or category caps.
- Categories archived, not deleted when referenced.
- Quick form first; optional itemization. Mixed-category spending allocated to item categories; never double count.
- Items retain quantity, unit, unit price and actual signed line total; explicit fee/discount/tax adjustments; exact reconciliation required.
- Refunds linked negative transactions, counted on refund receipt date; partial refunds permitted with limits.
- SGD only; no account transfers.
- Dashboard remaining budget prominent; tabs Dashboard, History, Settings; floating Add action.
- External AI receipt workflow with JSON and CSV, category-aware prompt, editable validation review.
- Manual JSON backup and validated replace-all restore, CSV export, backup reminder.
- Offline after installation; future sync possible but not built. Functional clean UI; incremental tested vertical slices.

## Implementation conventions resolving ambiguities
- Incoming carryover in rollover week derives from previous week's positive remaining even if previous week was fixed mode.
- Initial onboarding week starts with zero carryover.
- Completed week configuration immutable through normal settings UI; old transaction edits may recalculate later balances.
- Refunds inherit inclusion at creation, then retain independently editable flag.
- Mixed-category refunds require original-item references to preserve category allocation.
- Historical category caps are effective-week records.
- Manual backup export is not equivalent to verified off-device backup.
