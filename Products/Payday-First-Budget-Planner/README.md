# UK Payday-First Budget Planner — Build Status

**Status:** BUILT + REMEDIATED + FINISHING PASS (executed 19–20 September 2026) + **PHASE 2 VISUAL PASS (executed 23 September 2026)**. Production Google Sheets workbook, functionally complete and visually polished. Ready for Boardroom verification/audit.
**Workbook:** https://docs.google.com/spreadsheets/d/1RQ9Gtg7CRIxQkbVIuCedftLj6LJJe3gtUIfN8lusWOU/edit
**Reports:** `01_Projects/UK-Payday-First-Budget-Planner/COMPLETION_REPORT_3.md` (finishing pass; supersedes _2 and _) · COMPLETION_REPORT_2.md (F01 remediation) · COMPLETION_REPORT.md (original build)
**NOTE:** 25/09/2026 is the workbook **Anchor Date**, NOT an execution date. All execution occurred 19–20 Sept 2026.

## What exists
- Exactly six canonical tabs: `1_Start_Here`, `2_Paycheck_Hub`, `3_Bills_Schedule`, `4_Daily_Tracker`, `5_Sinking_Funds`, `6_Engine_Lookups` (hidden + protected).
- 110-period engine (`6_Engine_Lookups!A5:H114`) supporting Weekly/Fortnightly/Four-Weekly/Monthly with ID prefixes **W/FN/FW/M** and `Seq_In_Year` annual reset. Final period under Weekly: PP-2028-W43 (27/10/2028 → 02/11/2028).
- 15 named ranges (13 mandatory + `Opening_Bank_Balance`, `Opening_CC_Balance`), all verified by name-resolution readback.
- Six-metric Paycheck Hub; canonical Safe-to-Spend equation.
- Stored `Pay_Period_ID` immutability + advisory `Suggested_Pay_Period_ID` + mismatch diagnostic.
- Dynamic bill allocation (live projection, no matrix); sinking funds; CC Liability/Reserve/Available_Cash.
- `#E8F0FE` editable inputs; 10 protected ranges (all read back, enforced), real `ONE_OF_LIST` validation.
- Advisory suggestion / mismatch / fund-movement helper formulas cover tracker **rows 5–20 only** (the seeded block), NOT all 300 rows — by design, not a defect.

## Finishing pass (second Themis audit, M01/M02/M03)
- **M01 (bill containment) FIXED:** `3_Bills_Schedule!F5:F20` now gates the approximate MATCH on `Period_Starts` with an explicit `AND(PeriodStart<=DueDate, PeriodEnd>=DueDate)` containment test; a due date after Period 110 end returns blank instead of falsely resolving to the final period. Verified: active period PP-2028-W43, day-3 bill → blank (was PP-2028-W43); day-2 → 02/11/2028 = PeriodEnd → PP-2028-W43; day-27/28 in-range → PP-2028-W43.
- **M02 (suggested-period containment) FIXED:** `4_Daily_Tracker!G5:G20` same containment gate. Verified: 03/11/2028 → blank (was PP-2028-W43); 24/09/2026 pre-anchor → blank; 25/09/2026 → PP-2026-W01; 27/10/2028 & 02/11/2028 → PP-2028-W43.
- **M03 (traceability):** completion report now carries the ORIGINAL Hermes Test 1–28 numbering/names (recovered from the 19-Sept build prompt in the DSH session log). Boundary tests reported separately as "Additional Finishing-Pass Verification".

## Phase 2 visual pass (23 September 2026)
Customer-facing beautification applied in place; **COMPLETE WITH DOCUMENTED EXCEPTIONS**. Report: `01_Projects/UK-Payday-First-Budget-Planner/PHASE2_EXECUTION_REPORT.md`; baseline: `PHASE2_BEFORE_STATE.md` (same folder).
- Applied: approved typography (16/12/10/20pt hierarchy, Arial), `#E8F0FE` inputs + `#DADCE0` borders, `#F8F9FA` outputs, semantic status colours with text labels (Active amber / Paused grey / CHECK red / STS-negative alert), `#3C4043` table headers, `#F1F3F4` KPI cards, quick-start banner + 3-step callout, hyperlinked navigation on all seven required routes, `6_Engine_Lookups` do-not-edit warning.
- `Net_Safe_To_Spend` is the dominant 20pt KPI; sinking funds gained an explicit `£70 / £1,000` progress text column (additive, outside protected ranges).
- Verified unchanged: 100% of protected formulas, all 15 named ranges, all 10 protected ranges, six-sheet structure, 110-period engine (`PP-2028-W43` tail intact), STS = £594.81.
- NOT done (boundaries): demo tab (would be a 7th tab — pending CEO authorisation), Quick-Start PDF (no authorisation). Column-width/freeze tools unavailable in the verified surface. Amber pair `#B06000`/`#FEF7E0` measures 4.32:1 (spec palette mandated; recorded).

## Open decisions (do NOT silently resolve)
1. **Bill Frequency / Status semantics — UNRESOLVED.** The Hermes spec lists Frequency and Status as bill-model fields but the §23.1 six-step allocation process never filters on either; there is no "active Monthly bills only" wording in the approved spec. Current behaviour (all frequencies allocate by constructed due date; `Status="Active"` gates the due-date construction) is an implementation choice where the spec was silent. Minimum decision: which Status values participate in allocation, and whether Frequency is functional or informational.
2. **Debt Payoff scheduling: Option A (bill model) vs Option B (sinking-fund model) — unresolved.** Accounting verified (reduces Liability and Bank, not Reserve, no direct STS effect); no payoff bill/fund created; Committed Cash link not wired. Does not block any Hermes test.

## Honest limits
- UI-level manual data entry not directly testable via API (F01 proof = `strict:false` readback + non-listed name stored and classified).
- Protection configured + read back, but cross-user enforcement untestable (credential is the workbook owner).
- `val_ActivePayID` is free-typed (range-sourced `ONE_OF_RANGE` rejected by Google); strict `ONE_OF_LIST` on all genuinely closed fields.
- Nango `update-values` needs `valueInputOption=USER_ENTERED` for formulas; `RAW` stores `=...` as text and destroys spills.
- Ships with labelled sample data (T01–T16, 8 bills, 3 funds).
