# Vibe Browser + Make: Workflow Learning Ledger

**Project:** Aletheia Emergence  
**Purpose:** Preserve practical lessons from using Vibe Browser MCP tools exposed through Make, including reliable browser workflow patterns, scenario behaviour, verification, blockers, and credit planning.  
**Status:** Living working record. This document records lessons and evidence; it does not independently authorise future browser actions, storefront edits, or publication.  
**Last updated:** 2026-10-12

## 1. Why this ledger exists

Vibe Browser scenarios are the current browser “eyes and hands” for this workflow. They are new to both Tilly and Metis. Recording observed behaviour as it happens should reduce repeated discovery, avoid unnecessary scenario calls, and help estimate the cost of small, medium, and larger workflows across a monthly Make allowance.

Keep three things separate:

- **Observed fact:** directly supported by a tool result or fresh inspection.
- **Working hypothesis:** plausible explanation that has not been established.
- **Estimate:** planning guidance, not a measured charge.

Update this document as workflows are tested. Do not turn an isolated success into a guarantee.

## 2. Operating principles

1. **Use the least costly route that is reliable enough.** Prefer existing, functionally tested scenarios over adding new wrappers. Spend credits where they materially improve delivery or verification; do not omit necessary checks merely to save a small amount.
2. **Separate reading from Vibe actions.** Use direct Opera for ordinary page reading, page structure, research, and independent verification whenever it can establish the needed facts. Use Vibe Browser through the existing Make wrappers for interactions that require the Brave/Vibe session. Do not pay for a duplicate read that Opera can reliably provide. Match the Brave target by URL and page identity, then use fresh Vibe-specific identifiers. Never reuse Opera IDs in Vibe or assume the two browsers share state.
3. **Refresh selectively, not reflexively.** Refresh only when freshness matters or evidence suggests the page is stale. Refresh each browser independently; a refresh in Opera does not synchronise Brave, and vice versa. Avoid refreshing an editor with unsaved work. After a Vibe action, verify through Opera where possible, otherwise use a fresh Vibe view when necessary.
4. **One bounded action at a time.** Inspect the target, perform one action, verify its result, then continue.
5. **Keep scenarios inactive between individual runs.** The standard Make wrapper workflow has been: activate the one required on-demand scenario, run it once with explicit inputs, deactivate it, and verify the scenario is inactive when safe and permitted.
6. **Stop on HTTP 429.** Do not make any further Make calls, including deactivation, readback, or cleanup calls, until Tilly provides a fresh trigger. Report the state as uncertain if it cannot safely be checked.
7. **Do not bypass safety or permission blocks.** A safety block is not the same as HTTP 429, but it is still a boundary. Record the exact point of blockage, do not route around it, and seek Tilly’s direction before further attempts.
8. **A successful Make response is not proof of the browser outcome.** Verify the actual page, saved state, and expected content.
9. **Keep browser page IDs, tab IDs, UIDs, and sessions distinct.** Use the identifier expected by the specific Vibe tool. Do not assume an identifier from one tool or browser connection is valid for another.
10. **Treat dynamic editor UIDs as provisional.** A fresh snapshot helps identify controls, but the recent UID mismatch shows that a reported click result must be checked against the intended outcome.
11. **Do not disturb unrelated tabs or sessions.** The browser can contain work from other projects. Do not close or navigate their pages as part of a Payhip task.
12. **No publication by implication.** The Payhip storefront remains unpublished unless Tilly explicitly authorises publication. The separate product-page completion task remains deferred until photography, final revision, and consumer testing are complete.

## 3. Scenario inventory relevant to the current workflow

The twelve Vibe Browser wrappers are documented in the companion baseline in `dragonzheartzz/Boardroom-Metis-Private`:

- `list_pages` — scenario `7895104`
- `fill` — `7895212`
- `take_snapshot` — `7895535`
- `take_screenshot` — `7895536`
- `web_fetch` — `7895537`
- `navigate_page` — `7895541`
- `new_page` — `7895542`
- `switch_to_page` — `7895544`
- `scroll_page` — `7895545`
- `click` — `7895620`
- `wait_for_url` — `7895623`
- `wait_for` — `7895626`

These IDs belong to the Make/Vibe Browser workflow, not Opera or Opera Neon. The separate scheduled scenario `Vibe Browser Tool Access` (`7893732`) is outside this wrapper set and must not be touched as part of these tests.

The companion baseline currently records functional evidence for ten tools; `web_fetch` is domain-dependent and `take_screenshot` has not delivered a usable image through the wrapper. `take_snapshot` is the practical default for page content, structure, URLs, and actionable UIDs.

## 4. Editor and preview workflow

### Roles of the two pages

- **Editor:** make an authorised change and verify it was applied/saved using the editor’s own controls and visible state.
- **Preview:** inspect the draft as rendered. After a change is saved and verified in the editor, refresh the separate preview page, then snapshot it to check whether the expected result appears.

Do not refresh the editor as a routine step. The editor may hold unsaved state, and refreshing could discard a change. The approved working pattern is specifically to refresh the **preview tab**, after the editor save has been confirmed.

### Recommended sequence for a content change

1. Take a fresh snapshot of the editor and establish the current page and intended control.
2. Perform one authorised interaction. For nested menus or long panels, use the page structure and scroll when needed rather than assuming a visible top-level button is the save control.
3. Find and use the correct save/apply control. If the UI is ambiguous, inspect further rather than guessing.
4. Verify the changed value or state in the editor after saving.
5. Switch to the separately identified preview page.
6. Use `navigate_page` with navigation action `reload` on that preview page.
7. Take a fresh preview snapshot and verify the specific expected change.
8. If the preview does not reflect the edit, do not repeat the edit blindly. Investigate whether the save completed, the preview requires another step, or the target page is wrong.
9. Deactivate the scenario after its run and confirm the relevant wrapper is inactive when safe and permitted.

This sequence is a working procedure to validate as we go, not a guarantee that every Payhip editor component saves the same way.

## 5. Verified workflow lessons

### 2026-10-12 — Preview refresh test

**Objective:** Test Tilly’s suggestion to refresh the preview after a confirmed editor save, and establish whether the existing wrapper can do so without creating a new scenario.

**Scope:** Read-only inspection and refresh of the existing Payhip preview. No content edit or publication.

**Target:** Preview page ID `1509476798`, titled `Aletheia Emergence - Payhip`, at the Payhip builder-mode preview URL.

**Tools used:**
- `list_pages` — `7895104`
- `take_snapshot` — `7895535`
- `navigate_page` — `7895541`

**Actions and evidence:**
- A fresh page listing returned 11 browser pages and identified both the Payhip editor (`1509476797`) and preview (`1509476798`). Other project tabs were left untouched.
- A Markdown snapshot of preview page `1509476798` identified the expected Payhip title and URL, and showed the page as a private draft rather than the public storefront.
- `navigate_page` was run with `pageId=1509476798` and `navigationType=reload`. The tool returned: `Refreshed page: https://payhip.com/AletheiaEmergence?builder_mode=1&environment_id_encrypted=…`.
- A second snapshot of the same preview page returned the same title, URL, visible draft content, and page-state hash as the first snapshot. This is consistent with a successful refresh of an unchanged page. It does not yet prove how a newly saved edit propagates into preview.
- The editor snapshot attempted afterwards was blocked by OpenAI safety checks. This was not an HTTP 429. The snapshot wrapper was then deactivated successfully.
- A fresh scenario-state read confirmed `list_pages`, `take_snapshot`, `navigate_page`, and `click` were inactive, on-demand, and had zero incomplete executions at that checkpoint.

**Outcome:** PARTIAL. The existing `navigate_page` wrapper can refresh the known preview page, and the preview can be inspected before and after. The end-to-end “save editor change → refresh preview → observe new content” test remains pending.

**Credit accounting:** The four successful browser runs in this test sequence were `list_pages`, preview snapshot before refresh, preview reload, and preview snapshot after refresh. Their exact Make credit and data-transfer charges have not been reconciled from execution records in this entry. Historical wrapper testing recorded one credit per stored execution, but that is a planning reference, not a substitute for reading the current run records. The safety-blocked editor snapshot is not counted as a successful browser execution. Do not infer the exact account charge from the number of scenario-management calls.

**Reusable lessons:**
- No new refresh scenario is needed for the observed use case; `navigate_page` already exposes a `Reload` action.
- Use refresh on the preview tab after confirming the editor save, not as a default action on the editor.
- Snapshot and refresh are complementary: refresh requests a reload; snapshot establishes what content is visible afterwards.
- A matching before/after snapshot of unchanged content validates the basic refresh path, but not propagation of new edits.
- Safety blocks must be recorded distinctly from HTTP 429s. Do not work around them.

**Next useful validation:** During the next authorised content edit, save and verify it in the editor, reload only the preview page, then confirm the exact changed text or state in a fresh preview snapshot. Do not perform that test until browser work has a fresh CEO trigger.

## 6. Identifier mismatch and shared-browser caution

A prior Payhip editor interaction produced an important unresolved discrepancy:

- A snapshot showed UID `A12` associated with an About page in the editor’s custom-pages panel.
- A subsequent click response reported that it clicked a link labelled “Video”.
- A separate click on UID `A1` had expanded the custom-pages panel as expected.

This does **not** establish the root cause. Possibilities include a stale/dynamic UID, a change in page state between snapshot and click, a page/session targeting issue, or behaviour in the editor itself.

Working safeguards:
- Do not trust a click response alone when the reported target label does not match the snapshot.
- Take a fresh snapshot immediately before any consequential interaction.
- Verify the resulting page or state afterwards.
- If the identifier/label mismatch recurs, stop the sequence and investigate rather than trying adjacent UIDs.
- The public Vibe documentation warns that enabling MCP External Control in multiple browser profiles on one relay may make targeting ambiguous. Do not change profile or relay configuration without establishing which session the current connection controls.
- Tilly may use a separate Brave browser, but that only improves isolation if the Vibe connection is confirmed to target the intended profile. Do not assume separation merely because another browser window is open.

## 6A. 2026-10-12 — Payhip cross-browser alignment and editor failure record

**Objective:** Resume authorised private-draft storefront work, align the same target URL in Opera and Brave/Vibe, and record tool failures. No publication authorised.

**Observed starting state and cross-browser alignment:**
- Direct Opera list_tabs succeeded. Editor tab 1396777966 was at https://payhip.com/builder/public; preview tab 1396777967 was initially on the Contact draft-preview URL.
- Vibe list_pages scenario 7895104 succeeded twice. Editor page 1509476797 was at the same https://payhip.com/builder/public URL; preview page 1509476816 was Contact draft preview.
- Opera tab 1396777967 was navigated to the exact Contact URL reported by Vibe, then to the exact About preview URL reported by a fresh editor snapshot. Opera confirmed the About preview URL and displayed “Empty Page — Create your beautiful page by adding sections.”
- Opera editor snapshot showed the builder in Main editing context, with existing homepage sections and Publish/Save controls. No Publish action was taken.
- Vibe editor snapshot (7895535, Markdown) succeeded and showed a Text Section editor. The heading field’s accessibility text appeared to contain the approved About heading plus body copy, but the body textbox still showed Payhip’s default placeholder. The embedded preview showed “Discuss your brand or product” and the default body placeholder. This is inconsistent with the intended About content and does not prove the body copy was saved.
- The editor’s page selector was opened successfully with click wrapper 7895620, UID A1. A fresh accessibility snapshot showed search field UID A63, About link UID A66, Contact A65, Main A67. Clicking About UID A66 failed internally, so the selected editor page could not be safely confirmed.

**Tool execution record:**
- PASS: Opera list_tabs, tab_content for editor/preview, and go_to_page for matching preview URLs.
- PASS: Vibe list_pages scenario 7895104 (two runs; wrapper deactivated after each).
- PASS: Vibe take_snapshot scenario 7895535 in Markdown for editor and accessibility-tree format once when selector was open (wrapper deactivated after each successful run).
- PASS: Vibe click scenario 7895620 clicking editor page-selector UID A1.
- FAIL / UNVERIFIED: Vibe take_snapshot scenario 7895535 returned “The tool failed internally” for the Contact preview page and later for the editor immediately after an internal click failure. Each wrapper was deactivated after the run.
- FAIL: Vibe click scenario 7895620 for About selector UID A66 returned “The tool failed internally”. Do not retry unchanged.
- FAIL: Vibe fill scenario 7895212 targeting body textbox UID A17 returned a runtime error: “Cannot attach debugger to tab 1509476797: another debugger session (DevTools, or a second automation run) is already attached. Close DevTools on this tab or wait for the other session to finish.” The fill did not execute. Wrapper was deactivated.
- No HTTP 429 was reported in this session. Do not describe these failures as rate limiting.
- Make usage/credit charges for this session were not reconciled; do not estimate a balance from scenario calls.

**Outcome:** PARTIAL / BLOCKED for editing. Cross-browser URL alignment was achieved and verified, but the editor’s current page context and body-field targeting remain unresolved. No verified About body update or save occurred. No evidence of publication; Publish was not clicked.

**Working hypotheses, not established causes:** The debugger-attachment error is consistent with a second debugger/automation session on the same Vibe tab, but this does not establish which session owns it or whether the concurrent connection test caused it. The mismatch between Main editor context, About preview URL, and placeholder content could reflect unsaved state, page-selection state, or an editor/preview quirk. Do not repair or repeat the edit until the active target is confirmed.

**Next step:** Before any further write, inspect the current editor and page selector with one fresh Vibe snapshot; if debugger attachment still blocks interaction, stop Vibe writes and resolve which connection owns the Brave tab. Keep Opera on the exact same URL as the Brave preview for independent verification. Once About is clearly selected, make one bounded body-field change, verify it in the editor, use the existing preview reload workflow, and verify the exact rendered text in Opera. Never publish without Tilly’s explicit approval.


## 6B. 2026-10-12 — Resumed after Payhip navigation confirmation

Tilly reported manually confirming the browser's “leave this page / unsaved changes” dialog and selecting “do not ask again”. This was a plausible explanation for prior navigation/state confusion, but causation is not established.

**Fresh verification:**
- Opera tab 1396777966 remained on https://payhip.com/builder/public. Its accessibility tree explicitly showed “You're currently editing a single custom page ‘Main’” in the first Opera read.
- Opera preview tab 1396777967 remained on the About draft URL and displayed “Empty Page — Create your beautiful page by adding sections”.
- Vibe list_pages wrapper 7895104 succeeded. Current editor page 1509476797 was https://payhip.com/builder/public; preview page 1509476816 was Contact preview.
- Vibe snapshot wrapper 7895535 succeeded and explicitly showed the editor context is now **About**, with “You're currently editing a single custom page ‘About’”. The About preview iframe remained Empty Page. This is a material improvement over the previous ambiguous Main selection, but the page still has no visible content sections.

**Actions and outcomes:**
- Click wrapper 7895620 on UID A2 successfully opened the editor's Add menu. Snapshot showed menu entries Section UID A3 and Custom Page UID A4.
- Clicking UID A3 did not select “Section”; the wrapper reported “Clicked button ‘#T’ via CDP”. A fresh snapshot showed a responsive preview option menu with “# Desktop” and “$ Mobile”. This demonstrates duplicate/unstable accessibility UIDs: A3 referred to different controls in different DOM regions or the click resolver selected the wrong element. No section was added.
- All wrapper scenarios used were deactivated after each run. No HTTP 429 occurred. No fill or save was attempted this session. Publish was not clicked.

**Outcome:** PARTIAL. The editor's selected page is now verified as About and the previous debugger attachment error did not recur during read-only snapshots or click wrapper runs. However, the About page remains empty, and the generic click wrapper cannot reliably target the Section menu item due to duplicate UIDs. Do not retry UID A3 unchanged.

**Next step:** Need a reliable, scoped selector/targeting route for the Section menu entry and subsequent section-picker choice, or use an approved UI action that resolves the visible element unambiguously. Keep Opera preview aligned to the exact About preview URL and verify every structural change there. Do not save/publish until the intended section and copy are visibly correct; never publish without explicit approval.

## 7. Credit planning and monthly estimates

The first-month planning allowance recorded for this work is **1,000 Make credits**. The purpose is to complete the storefront platform work and learn which reusable workflows are economical enough to reuse across projects. Actual account consumption must be distinguished from this project’s measured runs.

### Evidence currently available

| Measure | Current record | Interpretation |
|---|---:|---|
| Historical Vibe wrapper test set | 27 stored executions / 27 credits | Previously reconciled execution-history total; not the whole Make account |
| Historical data transfer for that test set | 65,058 bytes | Sum from those stored execution histories |
| Recent Payhip preview-refresh sequence | 4 successful browser runs | Exact credit/data-transfer total not reconciled in this entry |
| New refresh scenario required | No | Existing `navigate_page` wrapper supports reload |
| Remaining monthly allowance | Unknown | Must be read from the account’s current usage/billing view; do not calculate it from project-only tests |

Historical testing suggests a useful **rough planning assumption** of about one credit per wrapper execution, but it is not a guaranteed tariff for every future scenario or workflow. Scenario activation, execution, deactivation, state verification, and any additional inspection calls may create separate tool operations or costs; check actual usage records where available.

### How to estimate a workflow before running it

Use the following planning method, then reconcile against actual records:

1. Count expected browser executions (for example, one editor snapshot, one action, one editor verification, one preview reload, one preview snapshot).
2. Use the historical one-credit-per-execution figure only as a provisional estimate.
3. Add a modest allowance for ambiguous menus, scrolling, or a correction pass where the UI is known to be convoluted.
4. Keep scenario configuration/inspection and account-management overhead separate from browser execution costs when the records allow it.
5. After the task, record actual execution IDs, credits, data transfer, failed/blocked runs, and any manual intervention. Replace estimates with measured values.
6. Use the observed average from several comparable tasks to estimate small, medium, and large workflows. Do not extrapolate from one Payhip session alone.

A simple estimate can be expressed as:

`Estimated workflow credits = expected wrapper runs × provisional credits per run + contingency`

The contingency should be based on observed friction from comparable work, not an arbitrary fixed percentage. Never present the result as remaining account balance.

## 8. Result status vocabulary

- **PASS:** expected behaviour observed and supported by evidence.
- **PARTIAL:** some criteria met, with a material gap remaining.
- **BLOCKED:** no safe or authorised route to execute the next step.
- **FAIL:** the test executed but expected behaviour did not occur.
- **UNVERIFIED:** output is insufficient to establish the outcome.

A successful Make status does not automatically mean PASS for the browser-level objective.

## 9. Open questions

- Does a saved editor change appear after one preview reload, and how reliably?
- Which Payhip editor controls save immediately, require an explicit save button, or trigger a secondary menu?
- Can the exact click-UID mismatch be reproduced safely on a harmless target?
- What are the measured credit/data-transfer costs for the current Payhip workflow, including any retries or blocked attempts?
- How much overhead is attributable to scenario activation/deactivation and state checks versus actual browser runs?

Resolve these only when relevant to an authorised task. Avoid extra diagnostic runs without a clear decision they will inform.

## 10. References

- [Payhip Storefront and UK Payday-First Budget Planner specification](https://github.com/dragonzheartzz/Aletheia-Emergence/blob/main/docs/storefront/Payhip-Storefront-and-Payday-First-Budget-Planner-v1.md)
- [Payhip Platform Work Ledger](https://github.com/dragonzheartzz/Aletheia-Emergence/blob/main/docs/storefront/Payhip-Platform-Work-Ledger.md)
- [Vibe Browser MCP + Make testing baseline](https://github.com/dragonzheartzz/Boardroom-Metis-Private/blob/main/_Governance/20261011_VIBE-BROWSER-MAKE_Testing-Baseline_v01.md)
- [Vibe Browser MCP integration documentation](https://docs.vibebrowser.app/mcp-integration/)

---

**Governance note:** This is a record of lessons and cost evidence, not an execution authorisation. Tilly retains final authority. Continue to follow the current project specification and stop on HTTP 429 or any safety/permission block.
