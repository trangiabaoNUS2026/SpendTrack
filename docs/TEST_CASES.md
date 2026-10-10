# SpendTrack — Acceptance Tests
Use these as fixtures for automated tests; include negative/edge cases.

| ID | Scenario | Expected |
|---|---|---|
| B01 | Base 18000 cents, counted spending 10340 | remaining 7660 |
| B02 | Base 18000, spending 19500 | remaining -1500 and warning |
| B03 | Fixed current week, prior remaining +3000 | carryIn 0 |
| B04 | Rollover current week, prior remaining +3000 | carryIn 3000 |
| B05 | Rollover current week, prior remaining -1500 | carryIn 0 |
| B06 | Switch fixed to rollover midweek | current carryIn recalculated immediately |
| B07 | Switch rollover to fixed midweek | current carryIn 0 immediately |
| B08 | Edit older purchase | downstream rollover recomputed, config unchanged |
| B09 | Week Saturday 2026-10-10 through Friday 2026-10-16 | both endpoints included |
| B10 | Excluded expense 5000 | no weekly or category-cap impact |
| B11 | Refund -1000 dated current week | net spending reduced by 1000 this week |
| B12 | Refund causes net spending negative | remaining may exceed available; valid |
| C01 | Non-itemized expense 500, Eating Out | Eating Out allocation 500 |
| C02 | Itemized total 2340: Groceries 1340, Household 1000 | total 2340, correct category splits |
| C03 | Transaction 2340, item lines sum 2240 | save blocked; discrepancy 100 |
| C04 | Excluded itemized purchase | no category-cap allocation |
| C05 | Archive referenced category | old records remain readable |
| C06 | Change cap this week | past weeks retain old cap |
| R01 | Refund exceeds original purchase | reject |
| R02 | Two partial refunds together exceed item line | reject second |
| R03 | Edit original so refunds exceed original | reject edit |
| D01 | Save and reload expense | persisted |
| D02 | Valid backup restored into empty profile | all records, relations, configs equal |
| D03 | Invalid backup | existing data unchanged |
| D04 | Restore fails during transaction | no partial replacement |
| I01 | Unknown AI category | require manual assignment |
| I02 | Missing item price | editable review; block unresolved errors |
| I03 | Malformed JSON/CSV | reject without database mutation |
| S01 | Search 'beef' in item | parent supermarket transaction returned |
| P01 | Install and cache app, then offline | dashboard/history/entry usable |
