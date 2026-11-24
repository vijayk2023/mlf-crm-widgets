# WORKSPACE-PROGRESS.md - crm-giving-widgets

> **START OF EVERY SESSION:** follow the routine in the root `CLAUDE.md`. This file is the dashboard-level view; the detailed handoff is `sessions.md` (read it), change log `progress.md`, reasons `decisions.md`. **NOTE:** named WORKSPACE-PROGRESS.md because `progress.md` (the change log) already exists and Windows treats the names as the same file. **END:** update sections 1, 2, 3, add a section 5 entry, update `sessions.md`/`progress.md`/`decisions.md`, the `PROJECTS.md` row, commit in THIS repo (and the workspace repo for `PROJECTS.md`).

Last updated: 2026-11-24 (Session 5: P15 Account giving history live, D21; all 3,436 Accounts filled).

> **2026-11-24:** pending items added from the peer feedback 24 Nov (`Critical list to do.xlsx`). Work order across projects: `../crm-requirements-assessment/PEER-FEEDBACK-SEQUENCE.md`.

---

## 1. PENDING (what is left)

### SUGGESTED NEXT (show this with the table below when the user says "resume the Lifetime Giving project")
1. **P15 follow-up (read-only) - recommended first:** after the first night (25/11 02:30) check Schedules > Failures for `mlf_nightly_account_0230` and COQL `select COUNT(id) from Accounts where Giving_Last_Updated = '<that day>'` (expect 700). Runs of 700 took 135-210 s of the 300 s limit: if it ever times out, lower `maxAccounts` to 500 (your OK).
2. **P13 (read-only):** Contact nightly runs: Schedules > Failures and Logs; 18/11 (1,023 stamped) and 13/11 (1,374) looked low on 24/11.
3. **P16 / P17 (your decisions):** soft credit to a missing Contact/Account (manual vs automatic); soft credit amount field design (Q6 with Nel).
4. **P12 (you delete):** obsolete `mlf_fill_blank_giving_*` and `mlf_refresh_batch_*` schedules/functions.
5. **P14 (needs your export):** duplicate-contact report.
6. Then P18 (direct vs soft-credit totals, widget on Accounts), P19, P11, P6, P9.


| #         | Item                                                                                                                                                                                                                                                                                                         | Needs                                        | Where to look                                                             |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------- | ------------------------------------------------------------------------- |
| P13       | **NEW** Verify nightly runs (D20): Schedules Failures + function Logs; statement-limit risk on rolling runs of 800 heavy donors. First nights look fine by counts: 1,721 Contacts stamped 11/11, 1,617 on 12/11                                                                                              | Read-only, then your OK if the cap must drop | Setup > Schedules > Failures; `deluge/mlf_nightly_giving_refresh.dg`      |
| P12       | Delete the 11 `mlf_fill_blank_giving_*` schedules + `mlf_fill_blank_giving_run`, the `mlf_refresh_batch_1..5` schedules + functions                                                                                                                                                                          | User deletes                                 | Setup > Schedules; Functions                                              |
| P14       | **NEW** Duplicate-contact report (no-gift Contact sharing an email with a donor, e.g. Rosemary Miller 901783000002117004 / Rosie Miller 901783000002348456)                                                                                                                                                  | Your Contacts export, then offline report    | plan `C:\Users\VijayVasudevan\.claude\plans\warm-beaming-zebra.md` step 3 |
| P11       | Check Zoho Campaigns segment count after CRM sync                                                                                                                                                                                                                                                            | User                                         | Zoho Campaigns                                                            |
| P9        | Optional now: Contacts On Create rule, Lifetime Giving empty -> 0 (the nightly "changed" run already fills blanks within a day)                                                                                                                                                                              | User OK                                      | `sessions.md`                                                             |
| P6        | Remove `cert.pem`/`key.pem` from git tracking; decide on untracked `widgets/income_stream/`                                                                                                                                                                                                                  | User OK                                      | `sessions.md`                                                             |
| ~~P7~~ | Done: repo in sync with origin on 2026-11-24 | - | - |
| ~~P15~~ | **DONE 2026-11-24 (D21)** Account giving history: 10 Account fields; `mlf_aggregate_account_giving` + `mlf_nightly_account_giving_refresh` + `mlf_nightly_account_run`; Visa Europe test OK; 5 `blank` runs filled all Accounts (447 with totals, 2,989 at £0, 0 blank; sum £13,972,263.96 = sum of Account Donations); schedule `mlf_nightly_account_0230` Daily 02:30 from 25/11. Follow-up: first-night check (SUGGESTED NEXT 1). Not done: Lifetime Giving widget on Accounts (part of P18) | - | - |
| P16       | **NEW (#12 edge case)** When a soft credit points to a Contact or Account that does not exist yet, create it first (example: Visa Account `901783000002330577`). Decide whether that is manual (import step) or automatic                                                                                    | Your decision                                | `../crm-import-templates/` T20                                            |
| P17       | **NEW (#6)** Soft credit amount: a field for how much of a gift is soft credited (e.g. GBP 30 of GBP 50). Design decisions (one or several credits per gift; backfill existing soft credits at 100%?) are Q6 in the requirements project. Fix field names early so the import templates can carry them (T21) | Decision (Nel), then field creation OK       | `../crm-requirements-assessment/` Q6                                      |
| P18       | **NEW (#6)** Giving history separates direct giving from soft credits with a total for each, on Contacts and Accounts; Lifetime Giving widget shows both. Examples to test: Murray Birt / Deutsche Bank, Emma Slatter / Visa                                                                                 | After P15, P17                               | `widgets/`, `deluge/`                                                     |
| P19       | **NEW (#6)** Benevity matched gifts: the Donation starts on the company Account and is soft credited to the employee Contact; check the giving functions count it once per side                                                                                                                              | Read-only test after P18                     | `../crm-import-templates/` T20                                            |
| ~~P8~~    | DONE 2026-11-10: real `changed` run filled the last 93 blanks (COQL blank count 0)                                                                                                                                                                                                                           | -                                            | -                                                                         |
| ~~P10~~   | Dropped: `mlf_refresh_pages` redeploy not needed (nightly job replaces bulk batches)                                                                                                                                                                                                                         | -                                            | -                                                                         |
| ~~P2/P3~~ | Superseded: old batches 2-5 of 27/10 FAILED on the statement limit (Failures tab); the rolling refresh re-checks every Contact about every 2 weeks; clean-up = P12                                                                                                                                           | -                                            | -                                                                         |
| ~~P4~~    | Done: live shared function (hardened 2026-10-27) reads up to 2,000 donations by COQL paging; only Anonymous (6,871) is skipped                                                                                                                                                                               | -                                            | -                                                                         |
| ~~P5~~    | Mostly solved: rolling refresh fixes YTD within ~2 weeks after 1 January (optional: extra January runs)                                                                                                                                                                                                      | -                                            | -                                                                         |

---

## 2. DECISIONS MADE

| When | Decision |
|---|---|
| 2026-10-26 | D1-D18 recorded in `decisions.md` (shared function + thin wrappers; never-donors skipped; £0/negative gifts ignored; user deletes in Zoho). |
| 2026-11-09 | D19: never-donors get Lifetime Giving £0 (supersedes D14); backfill via `mlf_fill_blank_giving` on 11 one-off schedules created by Claude at user request. |
| 2026-11-10 | D20: nightly giving refresh (`mlf_nightly_giving_refresh`, modes changed/rolling, 3 daily schedules) replaces one-off batches; repo copy of the shared function synced with the live hardened version. |
| 2026-11-24 | D21: Account giving history: same 10 fields as Contacts, separate function `mlf_aggregate_account_giving` (live Contact function untouched), £0 for Accounts without gifts, all Accounts included (also Gift Aid, MLF USA/Australia), nightly refresh at 02:30; Donation workflow not changed. |
| 2026-11-03 | Folder joined the workspace as `crm-giving-widgets`; stays its own git repo; parent ignores it. |

---

## 3. CURRENT STATE
Functions (14): `mlf_aggregate_contact_giving` (live = hardened 2026-10-27 version, repo synced 2026-11-10), `mlf_calculate_giving_history`, `mlf_donation_trigger_aggregation`, `mlf_refresh_pages`, `mlf_refresh_batch_1..5` (obsolete), `mlf_fill_blank_giving` (keep), `mlf_fill_blank_giving_run` (obsolete), **`mlf_nightly_giving_refresh`** (standalone, D20), **`mlf_nightly_changed_run`**, **`mlf_nightly_rolling_run`** (schedule wrappers). Button "Calculate Giving History" (Contacts). Workflow "Auto-Update Giving Summary" active.
**Accounts (D21, 2026-11-24):** 10 giving fields (same API names as Contacts); functions `mlf_aggregate_account_giving` (standalone), `mlf_nightly_account_giving_refresh` (standalone; nightly/blank, -dry; cap 700), `mlf_nightly_account_run` (schedule). Schedule `mlf_nightly_account_0230` Daily 02:30 from 25/11/2026. 3,436 Accounts: 447 with totals, 2,989 £0, 0 blank. Accounts update the morning after a gift (no instant trigger).
**Schedules (active, Daily, never end, from 11/11/2026):** `mlf_nightly_changed_0100` (01:00, changed run), `mlf_nightly_rolling_0130` (01:30) and `mlf_nightly_rolling_0200` (02:00) (rolling run). Old one-off schedules (`mlf_fill_blank_giving_01..11`, `mlf_refresh_batch_1..5`) are off/completed; batches 2-5 actually failed on 27/10 (statement limit).
**Counts:** 2026-11-10: 0 blank Lifetime Giving (after the real changed run: 99 updated, 0 errors). 2026-11-11: 1,721 Contacts stamped; 2026-11-12: 1,617 stamped; blank still 0. Function credits 25,500/day, ~25,100 unused on 10/11.
**Note on "Last Donation Date empty":** ~9.8k Contacts (user's filter: Last Donation Date empty + Account empty) have no Donation linked at all (9,726 stamped £0; 9,773 are eTapestry contacts; user confirmed most are non-donors). Blank is correct for them; the rolling run re-checks them every ~2 weeks.

### Quick health checks (read-only)
1. `getRecord` Jillian Atkins (baseline 2026-11-10): lifetime £2,820, YTD £550, largest £500 (2026-06-25), first 2007-12-19, last £5 (2026-10-02), Giving_Last_Updated 2026-10-29. Changes only when she gives again.
2. James Wilson (baseline 2026-11-10): £11.99 lifetime/YTD/largest/last, first and last gift 2026-10-08 (no longer a never-donor). Never-donor test: Titus Gwemende 901783000003919050 (£0).
3. `git status -sb` in this folder.
4. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total is null` (0 since 2026-11-10; the nightly changed run keeps it 0).
5. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total = 0` (9,814 on 2026-11-10).
6. COQL `select COUNT(id) from Contacts where Giving_Last_Updated = '<yesterday>'` (about 1,600+ a day: 1,721 on 11/11, 1,617 on 12/11). 1,665 on 23/11. Much lower = a nightly run failed: check Schedules > Failures.
7. Accounts: COQL `Lifetime_Giving_Total is null` = 0; `= 0` 2,989 and `> 0` 447 (2026-11-24). Visa Europe 901783000002330577 baseline: £22,281.06 / YTD £553.69 / largest £4,000 2018-12-21 / first 2017-09-11 / last £553.69 2026-08-29.
8. COQL `Giving_Last_Updated = '<yesterday>'` on Accounts: 700 a night (447 with gifts + 253 oldest).

---

## 4. PROJECT-SPECIFIC LIMITS AND GOTCHAS
See `sessions.md` (Zoho function syntax rules, D16).

---

## 5. SESSION LOG

### Session 0 (2026-10-26, Sonnet 5.5) - before import to workspace
Full log in `sessions.md`. Widget enhanced, shared function refactor, pages 1-9 recalculated.

### Session 1 (2026-11-03) - Imported into workspace
Moved from `Zoho MLF Widget Projects/mlf-crm-widgets`; added CLAUDE/SCOPE/PROGRESS; nothing run in Zoho.

### Session 2 (2026-11-09, Opus 5.5) - Lifetime Giving £0 for never-donors
- Need: Zoho Campaigns segments on Lifetime Giving skip Contacts with a blank value. Found 8,894 blanks; 73 sampled had no donations.
- Built (repo, commits ec5a92f, ee33898 + docs): `mlf_fill_blank_giving`, `mlf_fill_blank_giving_run`; `mlf_refresh_pages` stampEmpty true (not redeployed). D19.
- Zoho (Chrome, user-approved): test on Titus Gwemende via the button (blank -> £0); created both functions; created 11 one-off schedules 10/11 01:00-06:00. Org guard checked twice (MLF).
- Discovery: batch schedules 1-5 completed 27/10 (P1 done).
- Tools: Zoho CRM MCP (getOrganization, COQL counts/samples), Claude in Chrome. No agents, no deletions, nothing blocked.
- Left: P8 verify run, P9 on-create rule, P10 redeploy, P11 Campaigns check, P12 user deletes schedules.

### Session 3 (2026-11-10, Opus 5.5) - Audit of the overnight £0 backfill (read-only)
- Org guard: `getOrganization` = `org20111555823` (MLF).
- Results: blank Lifetime Giving 8,894 -> 93; £0 1,013 -> 9,814 (+8,801); Contacts 21,491 -> 21,485 (6 fewer, likely deleted by users). The 93 left are the highest IDs (901783000002352285 up to 901783000003563069, incl. 10 created 08/10/2026): 11 runs x 800 cap = 8,800 < 8,894, so a capacity shortfall, not failures.
- Health checks: Jillian Atkins and James Wilson both changed because of new gifts (values look right); baselines updated in section 3 and `sessions.md`.
- Not done: schedule Logs/Failures (needs Chrome). No Zoho writes. Tools: Zoho CRM MCP (getOrganization, COQL x5 of which 2 failed on syntax/module name, getRecord x2). No agents, no deletions, nothing blocked.
- Docs: SUGGESTED NEXT block in section 1 and a resume rule in `CLAUDE.md`, so "resume the Lifetime Giving project" shows the pending list plus suggestions.
- Left: P8 finishing run (your OK), then P12, P9, P10, P11.

### Session 4 (2026-11-10, Opus 5.5; docs closed 2026-11-12) - Nightly giving refresh (D20)
- Started from the user's report: ~9.8k Contacts with an empty Last Donation Date (filter + Account empty). Found 9,726 have no Donation linked at all (blank is correct); recent donors were all correct (20/20 sample). Real gaps: imports with automations off, deleted/re-linked gifts, January YTD, 93 blanks, duplicates.
- Org guard `org20111555823` before every Zoho read/write session step (4 times).
- Repo: synced `mlf_aggregate_contact_giving.dg` with the live hardened version (copied from crm-donations-giving; Zoho modified time 27/10 05:15 PM confirmed); new `mlf_nightly_giving_refresh.dg`, `mlf_nightly_changed_run.dg`, `mlf_nightly_rolling_run.dg`; `.gitignore` adds data/, *.csv, *.xlsx. Commits d2e5a91, d504009 (local, not pushed).
- Zoho (Chrome, user-approved per step): created the 3 functions; dry runs (changed-dry: 99 to refresh; rolling-dry: 800); one real changed run: 99 updated, 0 skipped, 0 errors, 39 s; blanks 93 -> 0 (P8 done). Created 3 Daily schedules (01:00, 01:30, 02:00 from 11/11/2026).
- Bug: Zoho save error "Expecting ';'. Found 'rows' (Line 51)" was really `for each p in "1,2,3,4".toList(",")` 70 lines later; bisected by saving prefixes; fixed (list in a variable). Lesson added to `shared/zoho-crm.md`.
- Discovery: Schedules page said the 27/10 batches "Completed" but the Failures tab shows batches 2-5 stopped on the 200,000-statement limit. 207 old donations bulk-edited 10/11 16:12 by someone else (not us). Credits 25,500/day.
- Follow-up check 2026-11-12 (read-only): 1,721 stamped 11/11, 1,617 stamped 12/11, blanks 0. Failures tab for the new schedules not yet read (P13).
- Tools: Zoho CRM MCP (getOrganization x4, COQL ~30), Claude in Chrome (functions, schedules, credits, failures), Bash/Python for repo edits. Plan mode used (plan `warm-beaming-zebra.md`). No agents, no deletions, nothing blocked. Side effects: giving fields on 99 Contacts (correct recalculated values); test saves of trimmed code in the new (unused) function before the final save.
- Left: P13 verify, P12 user deletes, P14 duplicates (needs export), P7 push.

### Session 5 (2026-11-24, Opus 5.5) - P15 Account giving history (D21)
- Org guard `org20111555823` 3 times (start, before field creation, before the fill). Health checks: no drift (blank 0; £0 9,906; 1,665 stamped 23/11; test contacts match); 18/11 (1,023) and 13/11 (1,374) look low (P13).
- Discovery: Accounts had no giving fields; 447 of 3,436 Accounts have gifts (3,587 Donations); no Donation has both Contact and Account (no double count); largest "Gift Aid" 885 gifts.
- User decisions: 10 fields, separate function, £0 for no-gift Accounts, include every Account (D21).
- Zoho writes (user OK per step): `createFields` x10 on Accounts; 3 functions created via Chrome (code pasted with CodeMirror setValue, 0 errors); dry run `nightly-dry` 700 (447+253); Visa Europe test (all fields match offline calculation); 5 `blank` runs: 700/700/700/700/635 updated, 0 errors (135-210 s each); schedule `mlf_nightly_account_0230` Daily 02:30 from 25/11.
- Verification: 0 blank, 447 > 0, 2,989 = 0; total Lifetime on Accounts £13,972,263.96 = SUM(Received_Amount) of Account Donations; spot checks Gift Aid £503,101.61 (885 gifts), BP Foundation £1,211.07 (last 2026-02-03).
- Repo: dea4a31 (functions) + docs commits. Tools: Zoho CRM MCP (getOrganization x3, COQL ~25, getFields x3, createFields x1), Claude in Chrome. No agents, no deletions, nothing blocked. Side effects: Modified_Time changed on all 3,436 Accounts (Deluge updateRecord, no workflows fired).
- Gotcha: an Edit failed because an editor re-padded the pending table on disk; the next commit picked up that whitespace-only change.
- Left: first-night check of the Account schedule; P13; P16/P17 decisions; P18 widget on Accounts.
