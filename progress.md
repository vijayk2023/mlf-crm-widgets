# Progress / Change log

Newest first. One entry per meaningful change; commit labels follow `type(scope): summary`
(types `feat` `fix` `chore` `docs`; scopes `deluge` `widget` `crm` `docs`).
Per-session status and next steps live in [sessions.md](sessions.md); the reasoning behind changes is in [decisions.md](decisions.md).

## 2026-11-12
- `crm` check (read-only): nightly schedules ran: 1,721 Contacts stamped 11/11, 1,617 on 12/11; blank Lifetime Giving 0. Failures tab not yet read (P13).

## 2026-11-10 (Session 4)
- `fix(deluge)`/`feat(deluge)` d2e5a91, d504009: `mlf_aggregate_contact_giving.dg` synced with the live hardened version (deployed 27/10 from crm-donations-giving); new `mlf_nightly_giving_refresh` (changed/rolling, -dry), wrappers `mlf_nightly_changed_run`, `mlf_nightly_rolling_run`; `.gitignore` adds data/, *.csv, *.xlsx. D20.
- `crm` (Claude, Chrome, user-approved): created the 3 functions in Zoho; dry runs OK; real changed run 99 updated / 0 errors (39 s); blanks 93 -> 0 (P8 done). Created Daily schedules `mlf_nightly_changed_0100`, `mlf_nightly_rolling_0130`, `mlf_nightly_rolling_0200` (from 11/11/2026).
- `crm` discovery: Schedules > Failures shows `mlf_refresh_batch_2..5` (27/10) stopped on the 200,000-statement limit, although the list says "Completed".

## 2026-11-10
- `crm` audit (read-only, Zoho CRM MCP): overnight backfill result: blank `Lifetime_Giving_Total` 8,894 -> 93; = 0 1,013 -> 9,814 (+8,801); Contacts 21,485. Remaining 93 = highest IDs (11 runs x 800 = 8,800 cap). Schedule logs not yet read. No writes.
- `docs`: test-record baselines updated (Jillian Atkins £2,820 / YTD £550 / largest £500 2026-06-25; James Wilson £11.99 on 2026-10-08); SUGGESTED NEXT block in `WORKSPACE-PROGRESS.md`; resume rule in `CLAUDE.md`.

## 2026-11-09
- `feat(deluge)` ec5a92f, ee33898: `mlf_fill_blank_giving` (standalone backfill) and `mlf_fill_blank_giving_run` (schedule wrapper); `mlf_refresh_pages` now stamps never-donors (`stampEmpty` true). D19.
- `crm` (Claude, Chrome): read-only counts: 21,491 Contacts, 8,894 blank `Lifetime_Giving_Total` (all also blank `Giving_Last_Updated`), 1,013 at 0; 73 sampled blanks had no Donation records.
- `crm` test: button "Calculate Giving History" on Titus Gwemende 901783000003919050 (all giving fields blank before) -> Lifetime/YTD/Last amount £0, Giving_Last_Updated 2026-11-09. Left as is (desired result).
- `crm` (Claude, Chrome): created functions `mlf_fill_blank_giving` (Standalone) and `mlf_fill_blank_giving_run` (Schedule, code verified), and 11 one-off schedules `mlf_fill_blank_giving_01..11` on 10/11/2026 at 01:00, 01:30 ... 06:00 (Europe/London), all enabled. **Not yet run.**
- `crm` discovery: Schedules page shows `mlf_refresh_batch_1..5` all "Completed" 27/10/2026 (switched off), so the P1 bulk recalculation ran; spot checks (P2) still to do.
- **Not deployed yet:** `mlf_refresh_pages` change (stampEmpty true) is in the repo only.

## 2026-10-26
- `crm` (Claude): created Schedule-category functions `mlf_refresh_batch_1..5` in Zoho (syntax-checked, saved; **not run, not scheduled**). Batch 1 now starts at page 10 (pages 1-9 already done). Repo files synced. User will create the one-off schedules (guide section 6). D18 recorded.
- `docs`: sessions.md rewritten with a "START HERE" handoff for the next session (model, tools, rules, test records, Zoho syntax quirks); CLAUDE.md and project memory point to it.
- `crm` BULK RUN IN PROGRESS (Claude, via Functions > MLF Refresh Pages > Execute, 200 contacts per page, `stampEmpty` false so never-donors are skipped): **Contacts pages 1-9 done** (1,800 contacts: 1,521 updated, 279 skipped). p1: 69 upd/131 skip, 32s; p2-4: 525/75, 124s; p5-9: 927/73, 216s. About 0.22 s per donor contact. **Remaining: pages 10-108** (~19,800 contacts, ~75 min). Safe to rerun any page (idempotent). Function credits page showed no drawdown (25,460 of 25,500 unused).
- `docs`: setup guide now has a map of which function is used where in Zoho (button, workflow rule, schedules, widget, layout); D17 records the cleanup decision.
- `crm` (done by the user): created the single button "Calculate Giving History" (In Record, details page) calling `mlf_calculate_giving_history`; tested. `docs`: setup guide updated to the real UI labels.
- `crm` (done by the user): deleted the three old Contacts buttons, the six schedules and the old batch/nightly/button functions. Re-audit: 5 functions left; old `mlf_lifetime_giving_aggregation` unassociated; no buttons, no schedules; workflow rule intact.
- `crm` (done by Claude via browser, tested with Execute): created `mlf_aggregate_contact_giving` (standalone), `mlf_calculate_giving_history` (button) and `mlf_refresh_pages` (standalone, syntax-validated only, **not executed**); replaced the code of `mlf_donation_trigger_aggregation` with the wrapper (the live workflow rule now uses the shared calculation; old code is in the function's Revisions). Tests: Jillian Atkins (lifetime £2,300, YTD £30, largest £500 on 2025-12-18, first 2007-12-19) and James Wilson (no donations, stamped). Buttons NOT yet repointed (argument picker would not open in automation). Repo files corrected for Zoho syntax (D16).
- `docs`: read-only audit of all callers of the giving functions (docs/audit-2026-10-26.md). Found the live workflow rule "Auto-Update Giving Summary" -> `mlf_donation_trigger_aggregation` (old logic). Added draft wrapper `deluge/mlf_donation_trigger_aggregation.dg` (D15).
- `feat(deluge)`: refactor to one shared function (D12-D14): `mlf_aggregate_contact_giving` (renamed from `mlf_lifetime_giving_aggregation_all`), `mlf_calculate_giving_history` (button), `mlf_refresh_pages`, and `mlf_refresh_batch_1..5` as wrappers. Written in repo; **not yet deployed in Zoho**. Old batch/nightly schedules were switched off by the user.
- `crm`: found the Details button called a stale copy `mlf_lifetime_giving_aggregation_all1` (early return, no >£0 filter); replaced with the latest repo code (line 1 renamed). Verified on James Wilson (no donations). `docs`: added the line-1 naming rule to deluge/README.md.
- `test`: D9-D11 version deployed in Zoho; Jillian Atkins retest passed (user-confirmed).
- `feat(deluge)` / `feat(widget)`: first/last/largest ignore £0 and negative records, £0-only years dropped from the breakdown; function always writes (clears stale values, stamps Giving_Last_Updated) when a contact has no donations; widget shows "No donation received to date" only for calculated contacts (D9-D11). Repacked `dist`.
- `test`: verified on Jillian Atkins (178 gifts, 2007-2026): YTD £30, largest £500 on 2025-12-18 (tie with 2022-07-12 resolved to the later), first gift 2007-12-19. User confirmed it worked. Widget display also verified in the browser on Gillian Eatwell.
- `crm`: replaced the function behind the Contacts button **Calculate Giving History** (Record - Details, Standard) with the repo version; previous code not kept. Verified on Gillian Eatwell: YTD, largest and first-gift fields populated correctly (single £50 gift, 2026-02-23).
- `feat(deluge)`: return message now includes YTD, largest and first gift so a run shows at once whether the new code is live.
- `docs`: added progress.md, decisions.md, sessions.md, CLAUDE.md, docs/zoho-setup-guide.md.
- `chore(widget)` 008c545: repacked `widgets/lifetime_giving/dist/lifetime_giving.zip` from `app/` (was the 17 Sep build).
- `feat(widget)` b62c92f: Lifetime Giving widget shows YTD, Gift Aid, Last gift, Largest gift (amount + date) and First gift in a 2x2 grid; `renderWidget` now takes one object. Tiles for largest/first/last hide when their date is empty.
- `feat(deluge)` 3449d62: `mlf_lifetime_giving_aggregation_all` now writes `Giving_YTD_Total`, `Largest_Donation_Amount`, `Largest_Donation_Date`, `First_Donation_Date`; removed the duplicated `updateRecord` call.
- `chore(deluge)` edcd0e0: imported the live function verbatim as a baseline (`deluge/`).
- `crm`: created 4 custom fields on Contacts (currency x2, date x2) in the MicroLoan org, then moved to the Life Time Value section. Not in git (CRM config).
  IDs: YTD 901783000003879001, Largest amount ...009, Largest date ...017, First date ...025.

## Before 2026-10-26
- 11f1df6 fix: add ZohoCRM.min.js auth layer and correct SDK version to 1.0.5
- 03ed696 feat: switch widget to URL params (removes SDK dependency)
- 3f66214 chore: remove node_modules from tracking
- 5d1b737 chore: add root index.html to activate GitHub Pages
- 2bebe5d feat: lifetime-giving widget
