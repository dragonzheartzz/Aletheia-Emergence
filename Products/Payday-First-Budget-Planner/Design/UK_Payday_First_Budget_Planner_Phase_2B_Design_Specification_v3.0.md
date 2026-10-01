# UK Payday-First Budget Planner — Phase 2B Design Specification

## Visual Hierarchy, Accessibility & Presentation Refinement

**Version:** 3.0
**Author role:** Athena, Boardroom Project Planning Specialist
**Requested by:** Metis, Primary Assistant (see `02-Athena/UK_Payday_First_Budget_Planner_Phase_2B_Design_Planning_Request.md`)
**Status:** CEO-approved for repository storage and Prompt Forge commissioning. Stored under the governed Boardroom specialist handoff workflow in `Work/03_Handoffs/`.
**Execution status:** This document is **not** an execution authorisation. No workbook modification is authorised by it. Any execution package must pass through the established specialist and audit pipeline (Argus pre-execution audit, Themis post-execution audit).
**Incorporates:** CEO decisions Q1–Q9.

---

# PROJECT PLAN

**Objective:**
Make the existing planner calmer, clearer and more accessible for a new user, using the smallest effective changes.

**Decision / outcome:**
CEO approval of this specification for repository storage and Prompt Forge commissioning (given).

**Project intent:**
Improve the visual presentation, clarity, accessibility and usability of the existing Google Sheets planner without redesigning its financial engine or architecture. The result should be a calm, coherent customer experience, not a decorative redesign or a dashboard.

**Scope:**
Presentation and customer-facing-text refinement of the five customer-facing sheets: `1_Start_Here`, `2_Paycheck_Hub`, `3_Bills_Schedule`, `4_Daily_Tracker`, `5_Sinking_Funds`. This covers layout, column sizing, number display, static visible labels, onboarding text and the input-cell border cue.

**Out of scope:**
- Palette redesign, masculine/feminine variants, dashboards, charts, new features.
- Formula, financial-logic, named-range or architecture changes; `6_Engine_Lookups`.
- Excel-specific redesign or optimisation; customer delivery format (deferred to Phase 3).
- Currency or language features. The launch product is UK-focused (GBP, UK English), and whether to support multiple currencies or languages is a future product/marketing decision. No currency-setting feature is added or changed.
- Post-sale documentation and a future interactive AI tutorial/support system. These are future concepts only and are not requirements.
- A formal first-time-user test (remains CEO post-completion validation, not a gate).

## Requirements

**Input versus calculated cells (Q1, Q7)**
- **R-A1.** Editable input areas carry a clear, consistent **bold black border** as a non-colour cue. Calculated and system cells do not.
- **R-A2.** Form of the cue by sheet:
  - `4_Daily_Tracker`: a bold structural border around the editable input block, not individual borders on each cell.
  - `1_Start_Here`, `3_Bills_Schedule`, `5_Sinking_Funds`: the same bold black border on the individual input cells, as decided under Q1.
- **R-A3.** The existing palette and fills stay. There is no palette redesign. The treatment is restrained and consistent, with no other decorative effects.
- **R-A4.** Text/background pairs continue to meet at least 4.5:1 contrast. The pairs measured already pass, so this is a "don't regress" requirement.
- **R-A5.** Colour is never the only carrier of meaning. The existing text cues (✓/!, CHECK, ALERT, Active/Paused) already satisfy this.
- **R-A6.** No formal accessibility-compliance claim is made unless separately verified.

**Legibility of financial values (Q3, Q8)**
- **R-D1.** `###` does not appear in important customer-facing financial fields during normal supported use.
- **R-D2.** This is achieved by column widths and/or display formatting only. No formula, financial logic or architecture is changed to fix width.
- **R-D3.** The normal test envelope tops out at **£100,000**. This is an internal design/formatting test value only. It is not a customer-facing limit, product ceiling, supported-income claim or marketing statement, and it must not appear in the workbook or product copy. £999,999.99 is used only if a specific technical reason emerges.

**Customer-facing labels (Q4)**
- **R-L1.** Static customer-facing labels and help text may be rewritten in plain English where that improves comprehension. The wording must accurately describe the underlying value.
- **R-L2.** Customer-facing text does not needlessly expose implementation terms (underscored field names, colour hex codes, raw URLs).
- **R-L3.** Internal identifiers, named ranges, formula references, lookup structures and other functional dependencies are unchanged.

**Onboarding (Q5, Q9)**
- **R-O1.** Quick Start and How-to-use are consolidated into **one first-time-user onboarding flow**: one place, one sequence, no competing instruction systems.
- **R-O2.** The consolidation is an assessment, not a deletion. Necessary information is preserved, duplication is removed, and the result reads as one journey.
- **R-O3.** The stale "Set Currency" reference in Quick Start step 1 is removed. Its wording becomes about frequency and the other real settings only.
- **R-O4.** The merged sequence also reconciles the remaining inconsistencies found: opening balances appear in Quick Start but not in How-to-use; setting the Active Pay Period ID appears in How-to-use but not in Quick Start; and `2_Paycheck_Hub` refers to a hard-coded cell address ("cell B11").
- **R-O5.** The workbook stays self-explanatory enough to begin correctly, without instructional density.

**Proportionality and preservation**
- **R-X1.** Prefer the smallest effective change. Every change traces to a requirement above.
- **R-P1.** All formulas, named ranges, validation lists, conditional-formatting rules, hidden/protected structures and stored values are unchanged unless separately authorised as a technical change.

## Constraints / invariants

Financial logic; formulas; Pay Period ID behaviour; transaction accounting; workbook architecture; input/output behaviour; hidden engine structure; the palette.

Two constraints from the file review apply directly to label changes:
- **Text that formulas read is a functional dependency, not a label.** Examples are the Tracker categories and payment methods (`Income`, `CC Payment`, `Debt Payoff`, `Transfer`, `Credit Card`, `Debit/Cash`), the status values (`Active`, `Paused`), the pay-frequency values and fund names. Dropdown option values must not be reworded.
- **Text generated by formulas is not a static label.** Examples are "✓ Set" / "! Required", "CHECK" and the overspend alert. Conditional-formatting rules key off the first characters of these messages. Rewording them needs a formula edit, so it is out of scope unless separately authorised.

Also: the onboarding restructure is limited to text-only areas. It must not move input or calculated cells that named ranges or formulas point to.

## Assumptions

- **A-1.** Google Sheets is the authoring environment and source of truth. The `.xlsx` reviewed is an export.
- **A-2.** Findings seen only in the export are checked against the master before they become changes.
- **A-3.** Athena has no access to the Google Sheet, so all observations come from the exported file and a LibreOffice render.
- **A-4.** Input cells are those carrying the input fill in the export. The exact input cells, including the extent of the `4_Daily_Tracker` input block (currently columns A–F of the ledger rows), must be confirmed in the master.

## Findings, classified against the Google Sheets master

| # | Observation | Classification |
|---|---|---|
| 1 | Customer-facing columns at default width, so long labels are cut off | Probably genuine. Confirm in master |
| 2 | Explanatory text beside working fields clipped by occupied neighbours | Probably genuine (follows from 1). Confirm |
| 3 | `###` on `Net_Safe_To_Spend` and `TOTAL RESERVE` (20pt, default width) | Reported in the original request, reproduced in render. Confirm how it appears in Sheets |
| 4 | Repeated raw web addresses in navigation cells | Likely export artefact. Confirm |
| 5 | Title and band fills covering only one cell | Likely export artefact (lost merges). Confirm |
| 6 | `£0.00` repeated on unused `Current_Balance` rows in `5_Sinking_Funds` | Genuine |
| 7 | Internal field names and a hex code in customer-facing text | Genuine (static text) |
| 8 | Two overlapping onboarding systems, with the stale currency step | Genuine. Decided under Q5 and Q9 |
| 9 | Sheet protection absent in the export, though the text says calculated cells are protected | Likely export loss. Confirm in master |

## Dependencies

- Master verification of the "Confirm" items.
- Daedalus review only if any change is found to touch functionality.
- CEO approval of this specification (given).

## Risks

- **RK-1.** Rewording text that formulas depend on would break the workbook.
- **RK-2.** The onboarding restructure could move cells that named ranges or formulas point to.
- **RK-3.** Acting on export-only findings could "fix" problems that don't exist in Sheets.
- **RK-4.** A structural border around the ledger block might visually merge with the existing table styling. The CEO will assess the result personally during final stress-testing.
- **RK-5.** Scope creep towards a redesign.

...