# Sessions

**Read this first.** It has the current state, what to do next, and everything a new session needs. At the end of every session: update "Current state" and "Next steps", add a session entry below, add to `progress.md` (change log) and `decisions.md` (new decisions only), commit, and push only when the user says so.

---
## START HERE (for a new session)

**What this project is:** a Zoho CRM widget plus the Deluge functions behind it for the MicroLoan Foundation (MLF). The "Lifetime Giving" widget on a Contact shows: lifetime total, YTD, Gift Aid, last gift, largest gift, first gift, and giving by year. The numbers are stored on Contact fields and written by Deluge functions. Widget code is in `widgets/lifetime_giving/`; Deluge in `deluge/`; Zoho how-to in `docs/zoho-setup-guide.md`; why-we-did-it in `decisions.md`.

**How the previous session ran (2026-11-10):** model **Claude Opus 5.5** (`claude-opus-5-5`), Claude Code in a Windows terminal (earlier: Sonnet 5.5 on 2026-10-26, Opus 5.5 on 2026-11-09). The user is non-technical-friendly: explain plainly, give numbered steps, keep answers short, show a table when comparing.

**Tools the previous session used (re-check they are connected):**
- **Zoho CRM MCP** (`mcp__claude_ai_Zoho_CRM__*`) connected to the **MicroLoan org**, EU data centre. Good for `getFields`, `getRecord`, `executeCOQLQuery` (COQL has no `group by` on lookups), `createFields`. It **cannot** see functions, buttons, workflows or schedules.
- **Claude in Chrome** (`mcp__claude-in-chrome__*`, load via ToolSearch) to read and edit Zoho Setup pages in the user's logged-in browser. Org URL base: `https://crm.zoho.eu/crm/org20111555823/`. Useful paths: `settings/functions?viewId=901783000000653181`, `settings/modules/Contacts/buttons`, `settings/workflow-rules`, `settings/schedules`, `settings/functions/credits`.
- GitHub repo `vijayk2023/mlf-crm-widgets` (branch `main`), `gh` CLI available.

**Rules agreed with the user (do not break):**
1. **Never delete anything in Zoho**; the user does deletions. Read-only audits are fine.
2. Do not push to GitHub unless asked. The first push attempt was blocked by the permission classifier; the user ran `! git push origin main` themselves, later pushes worked when explicitly asked. If a push is denied, give the user the command, do not work around it.
3. Deluge files in `deluge/` are the source of truth. Edit the file, commit, then paste into Zoho. Commit labels: `type(scope): summary` (`feat fix chore docs`; `deluge widget crm docs`).
4. Before ending a session update this file, `progress.md`, `decisions.md`.
5. Do not try to get around a tool's blocked output (the JS tool blocks output that looks like code/cookie text); read via screenshots instead.

**Zoho syntax rules learned (D16):** function signature must be on line 1 (comments go inside the body); argument types are only `string,int,date,float,bool,map`; lists are passed as comma-separated strings. Setting editor code reliably: `document.querySelector('.CodeMirror').CodeMirror.setValue(...)` via the JS tool, then click in the editor and press `End space BackSpace` to trigger validation; error count shows bottom right. The Button config's argument-value picker would not open for Claude; the user maps `id` to Contacts - Contact Id by hand.

**Known-good test records (MLF org):**
| Contact | ID | Expected after recalculation |
|---|---|---|
| Jillian Atkins | 901783000002331815 | (2026-11-10) Lifetime £2,820; YTD £550; largest £500 on 2026-06-25; first gift 2007-12-19; last £5 on 2026-10-02; 2012 £0 year absent from chart. (2026-10-26 values were £2,300 / YTD £30 / largest 2025-12-18, before newer gifts.) |
| James Wilson | 901783000003579001 | (2026-11-10) One gift £11.99 on 2026-10-08: lifetime, YTD, largest, first, last all £11.99 / 2026-10-08. For a never-donor test use Titus Gwemende 901783000003919050 (£0). |
| Gillian Eatwell | 901783000002352993 | One £50 gift 2026-02-23: all of YTD, largest, first, last match it |

---
## Current state (2026-11-10) - READ THIS FIRST
- **Overnight £0 backfill ran (10/11 01:00-06:00) and was audited read-only:** blank Lifetime Giving 8,894 -> **93**; £0 Contacts 1,013 -> 9,814; total Contacts 21,485. The 93 left are the highest IDs, because 11 runs x 800 = 8,800 is just short of 8,894 (capacity, not errors). Schedule Logs/Failures not read yet.
- Nothing written to Zoho on 2026-11-10. The 11 schedules and `mlf_fill_blank_giving_run` still exist (user deletes after P8 reaches 0).
- On resume, show the pending table and the SUGGESTED NEXT block from `WORKSPACE-PROGRESS.md` section 1 (see the resume rule in `CLAUDE.md`).

## Current state (2026-11-09)
- **Lifetime Giving £0 backfill (D19) is scheduled, not yet run.** 11 one-off schedules `mlf_fill_blank_giving_01..11` run on **10/11/2026 01:00-06:00** (every 30 min) the Schedule function `mlf_fill_blank_giving_run` -> standalone `mlf_fill_blank_giving("0")` (~800 Contacts per run) -> `mlf_aggregate_contact_giving(id, true)`. Before: 8,894 blank of 21,491 Contacts.
- Bulk batches 1-5 (pages 10-108) show "Completed 27/10/2026" on the Schedules page: P1 ran (the user created the schedules). Spot checks still owed.
- `mlf_refresh_pages` in the repo now passes `stampEmpty` true; **the Zoho copy is still the old (false) version**.
- Test contact for D19: Titus Gwemende 901783000003919050 (was all blank; now £0, stamped 2026-11-09).

## Current state (end of 2026-10-26)
- **Repo:** all of this session's work is committed locally on `main`; the final push was denied by the permission check and is run by the user (`! git push origin main`). Run `git status -sb` first: if it says `ahead`, push is still pending. Untracked: `widgets/income_stream/` (diagnostic and export zips, not part of this repo yet).
- **Zoho fields (Contacts, "Life Time Value" section):** the 6 original giving fields plus `Giving_YTD_Total`, `Largest_Donation_Amount`, `Largest_Donation_Date`, `First_Donation_Date`.
- **Zoho functions (9):** `mlf_aggregate_contact_giving` (standalone, all logic), `mlf_calculate_giving_history` (button wrapper), `mlf_donation_trigger_aggregation` (automation wrapper), `mlf_refresh_pages` (standalone bulk loop), `mlf_refresh_batch_1..5` (schedule wrappers, **not scheduled yet**). Repo copies in `deluge/`.
- **Zoho button:** one, "Calculate Giving History" (Contacts, In Record, details page) -> `mlf_calculate_giving_history`. Tested by the user.
- **Zoho workflow:** "Auto-Update Giving Summary" (Donation, create or edit, all donations) -> `mlf_donation_trigger_aggregation`. Active; keeps contacts current as donations change.
- **Schedules:** none exist. The user will create five one-off schedules for the batch functions (see `docs/zoho-setup-guide.md` section 6).
- **Widget:** the new version (zip `widgets/lifetime_giving/dist/lifetime_giving.zip`) is uploaded and verified on a populated contact.
- **Bulk recalculation:** Contacts pages **1-9 done** (1,800 contacts; 1,521 updated, 279 skipped as never-donors) by running `mlf_refresh_pages` from the function editor. Pages 10-108 are covered by the batch functions: batch 1 = pages 10-20, batch 2 = 21-40, batch 3 = 41-60, batch 4 = 61-80, batch 5 = 81-108. Never-donors are skipped by design (D14).

## Next steps (in order) - updated 2026-11-10
1. **With user OK:** run `mlf_fill_blank_giving` with `0` once from the function editor (fills the remaining 93 blanks; one run takes up to 800). Then in Chrome: Schedules page (11 x Completed) and Logs/Failures of `mlf_fill_blank_giving_run`; COQL blank count (target 0); spot-check 3 of the 93 (K J Payling 901783000003563060, Toby Wines 901783000003544002, Cynthia Siwila 901783000002352285).
2. With user OK: Contacts workflow rule, On Create, `Lifetime Giving Total is empty` -> Field Update `Lifetime Giving Total = 0` (test on one test Contact; user deletes it).
3. With user OK: redeploy `mlf_refresh_pages` (stampEmpty true).
4. User: check the Zoho Campaigns segment count after the next CRM sync.
5. User deletes the 11 `mlf_fill_blank_giving_*` schedules and `mlf_fill_blank_giving_run` once done (keep `mlf_fill_blank_giving` for catch-up runs), plus the old `mlf_refresh_batch_*` schedules/functions if not wanted.

## Next steps (in order) - 2026-10-26 (steps 1 done per the Schedules page; step 2 still open)
1. **User:** create the 5 one-off schedules (guide section 6), staggered 30+ minutes apart, and let them run (about 8-10 min for batch 1, about 15-17 min for batches 2-4, about 25 min for batch 5, at roughly 0.22 s per contact).
2. **User or next session:** check each function's Logs/Failures and the Schedules page; spot-check 3-4 contacts across the page ranges (compare against the expected values method used for Jillian Atkins, via `getRecord` or COQL).
3. After a clean run, switch the schedules off or delete them and the five batch functions if not wanted (user deletes).
4. Later options: handle contacts with more than 200 donations (paging in `mlf_aggregate_contact_giving`); an annual January bulk run because YTD goes stale until a contact is recalculated; remove `cert.pem` and `key.pem` from git tracking; decide what to do with `widgets/income_stream/`.

## Known issues / not done
- `searchRecords(...,1,200)` reads only the first 200 donations per contact; a donor with more would get understated totals.
- YTD becomes stale each 1 January until a contact is recalculated (a donation edit or the button refreshes it).
- Never-donors show "No giving data yet, click Calculate" until someone clicks the button (then "No donation received to date").
- No list-view mass-action button exists (removed in the cleanup, D17).
- `cert.pem` and `key.pem` (local dev HTTPS) are committed under `widgets/lifetime_giving/`.
- External callers of the Zoho functions (Zapier, Make, REST) cannot be seen from Zoho's screens; none are known.

## Session log
### 2026-10-26 - Giving widget enhancements (Sonnet 5.5)
Done: added YTD / largest / first-gift to the widget and the calculation; created 4 Contact fields; moved Deluge into git; fixed £0/negative handling (D9) and the "no donation received" state (D10, D11); refactored to one shared function with thin wrappers (D12-D17); audited every caller of the old functions; deployed and tested the new functions in Zoho; replaced the donation-trigger workflow function; recalculated Contacts pages 1-9; created the five batch functions; wrote `docs/zoho-setup-guide.md` and `docs/audit-2026-10-26.md`.
Handed back to the user: create the schedules, run them, verify, clean up.
Next session: follow "Next steps" above.

### 2026-11-09 - Lifetime Giving £0 for never-donors (Opus 5.5)
Need: Zoho Campaigns segments on Lifetime Giving miss Contacts whose value is blank. Found 8,894 blanks (no donations in samples). Wrote `mlf_fill_blank_giving` + schedule wrapper, flipped `mlf_refresh_pages` to stamp never-donors (D19); tested on Titus Gwemende via the button; created both functions and 11 overnight one-off schedules in Zoho via Chrome at the user's request. Next: verify the run on 10/11, then the on-create workflow rule and the `mlf_refresh_pages` redeploy (user OK each).

### 2026-11-10 - Audit of the overnight £0 backfill (Opus 5.5, read-only)
Org guard OK. Blanks 8,894 -> 93, £0 1,013 -> 9,814. The 93 left are the highest IDs: 11 runs x 800 cap = 8,800, just short. Test-contact baselines updated (Jillian and James have new gifts). No Zoho writes; schedule logs not read. Next: with user OK, one editor run of `mlf_fill_blank_giving("0")`, then the user deletes the schedules, then P9 / P10.
