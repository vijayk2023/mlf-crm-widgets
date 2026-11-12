# WORKSPACE-PROGRESS.md - crm-giving-widgets

> **START OF EVERY SESSION:** follow the routine in the root `CLAUDE.md`. This file is the dashboard-level view; the detailed handoff is `sessions.md` (read it), change log `progress.md`, reasons `decisions.md`. **NOTE:** named WORKSPACE-PROGRESS.md because `progress.md` (the change log) already exists and Windows treats the names as the same file. **END:** update sections 1, 2, 3, add a section 5 entry, update `sessions.md`/`progress.md`/`decisions.md`, the `PROJECTS.md` row, commit in THIS repo (and the workspace repo for `PROJECTS.md`).

Last updated: 2026-11-12 (Session 4: nightly giving refresh D20 live; blanks 0).

---

## 1. PENDING (what is left)

### SUGGESTED NEXT (show this with the table below when the user says "resume the Lifetime Giving project")
1. **P13 (read-only) - recommended first:** check the nightly runs: Setup > Schedules > Failures (any "limit of Statement" on `mlf_nightly_*`?) and the function Logs of `mlf_nightly_giving_refresh`; COQL `Giving_Last_Updated = '<yesterday>'` (expect about 1,600+). If a rolling run hits the 200,000-statement cap: lower `maxContacts` for rolling to 400 and add rolling schedules at 02:30 and 03:00 (needs your OK).
2. **P12 (you delete):** the 11 `mlf_fill_blank_giving_*` schedules, `mlf_fill_blank_giving_run`, the `mlf_refresh_batch_1..5` schedules and functions (superseded by D20; keep `mlf_fill_blank_giving` and `mlf_refresh_pages` for manual catch-ups).
3. **P14 (needs your export):** duplicate report: export Contacts (Record Id, Full Name, Email, Lifetime Giving Total, Last Donation Date, eTapestry ID) and give me the path; I build `data/duplicates_review.xlsx` offline (data/ is git-ignored).
4. **P7 (you):** push the widgets repo: `! git -C projects/crm-giving-widgets push origin main`.
5. Then P11 (Campaigns count), P6 (pem files), P9 (optional on-create rule).


| # | Item | Needs | Where to look |
|---|---|---|---|
| P13 | **NEW** Verify nightly runs (D20): Schedules Failures + function Logs; statement-limit risk on rolling runs of 800 heavy donors. First nights look fine by counts: 1,721 Contacts stamped 11/11, 1,617 on 12/11 | Read-only, then your OK if the cap must drop | Setup > Schedules > Failures; `deluge/mlf_nightly_giving_refresh.dg` |
| P12 | Delete the 11 `mlf_fill_blank_giving_*` schedules + `mlf_fill_blank_giving_run`, the `mlf_refresh_batch_1..5` schedules + functions | User deletes | Setup > Schedules; Functions |
| P14 | **NEW** Duplicate-contact report (no-gift Contact sharing an email with a donor, e.g. Rosemary Miller 901783000002117004 / Rosie Miller 901783000002348456) | Your Contacts export, then offline report | plan `C:\Users\VijayVasudevan\.claude\plans\warm-beaming-zebra.md` step 3 |
| P11 | Check Zoho Campaigns segment count after CRM sync | User | Zoho Campaigns |
| P9 | Optional now: Contacts On Create rule, Lifetime Giving empty -> 0 (the nightly "changed" run already fills blanks within a day) | User OK | `sessions.md` |
| P6 | Remove `cert.pem`/`key.pem` from git tracking; decide on untracked `widgets/income_stream/` | User OK | `sessions.md` |
| P7 | Push this repo (commits d2e5a91, d504009 + docs are local only) | User pushes | repo |
| ~~P8~~ | DONE 2026-11-10: real `changed` run filled the last 93 blanks (COQL blank count 0) | - | - |
| ~~P10~~ | Dropped: `mlf_refresh_pages` redeploy not needed (nightly job replaces bulk batches) | - | - |
| ~~P2/P3~~ | Superseded: old batches 2-5 of 27/10 FAILED on the statement limit (Failures tab); the rolling refresh re-checks every Contact about every 2 weeks; clean-up = P12 | - | - |
| ~~P4~~ | Done: live shared function (hardened 2026-10-27) reads up to 2,000 donations by COQL paging; only Anonymous (6,871) is skipped | - | - |
| ~~P5~~ | Mostly solved: rolling refresh fixes YTD within ~2 weeks after 1 January (optional: extra January runs) | - | - |

---

## 2. DECISIONS MADE

| When | Decision |
|---|---|
| 2026-10-26 | D1-D18 recorded in `decisions.md` (shared function + thin wrappers; never-donors skipped; £0/negative gifts ignored; user deletes in Zoho). |
| 2026-11-09 | D19: never-donors get Lifetime Giving £0 (supersedes D14); backfill via `mlf_fill_blank_giving` on 11 one-off schedules created by Claude at user request. |
| 2026-11-10 | D20: nightly giving refresh (`mlf_nightly_giving_refresh`, modes changed/rolling, 3 daily schedules) replaces one-off batches; repo copy of the shared function synced with the live hardened version. |
| 2026-11-03 | Folder joined the workspace as `crm-giving-widgets`; stays its own git repo; parent ignores it. |

---

## 3. CURRENT STATE
Functions (14): `mlf_aggregate_contact_giving` (live = hardened 2026-10-27 version, repo synced 2026-11-10), `mlf_calculate_giving_history`, `mlf_donation_trigger_aggregation`, `mlf_refresh_pages`, `mlf_refresh_batch_1..5` (obsolete), `mlf_fill_blank_giving` (keep), `mlf_fill_blank_giving_run` (obsolete), **`mlf_nightly_giving_refresh`** (standalone, D20), **`mlf_nightly_changed_run`**, **`mlf_nightly_rolling_run`** (schedule wrappers). Button "Calculate Giving History" (Contacts). Workflow "Auto-Update Giving Summary" active.
**Schedules (active, Daily, never end, from 11/11/2026):** `mlf_nightly_changed_0100` (01:00, changed run), `mlf_nightly_rolling_0130` (01:30) and `mlf_nightly_rolling_0200` (02:00) (rolling run). Old one-off schedules (`mlf_fill_blank_giving_01..11`, `mlf_refresh_batch_1..5`) are off/completed; batches 2-5 actually failed on 27/10 (statement limit).
**Counts:** 2026-11-10: 0 blank Lifetime Giving (after the real changed run: 99 updated, 0 errors). 2026-11-11: 1,721 Contacts stamped; 2026-11-12: 1,617 stamped; blank still 0. Function credits 25,500/day, ~25,100 unused on 10/11.
**Note on "Last Donation Date empty":** ~9.8k Contacts (user's filter: Last Donation Date empty + Account empty) have no Donation linked at all (9,726 stamped £0; 9,773 are eTapestry contacts; user confirmed most are non-donors). Blank is correct for them; the rolling run re-checks them every ~2 weeks.

### Quick health checks (read-only)
1. `getRecord` Jillian Atkins (baseline 2026-11-10): lifetime £2,820, YTD £550, largest £500 (2026-06-25), first 2007-12-19, last £5 (2026-10-02), Giving_Last_Updated 2026-10-29. Changes only when she gives again.
2. James Wilson (baseline 2026-11-10): £11.99 lifetime/YTD/largest/last, first and last gift 2026-10-08 (no longer a never-donor). Never-donor test: Titus Gwemende 901783000003919050 (£0).
3. `git status -sb` in this folder.
4. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total is null` (0 since 2026-11-10; the nightly changed run keeps it 0).
5. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total = 0` (9,814 on 2026-11-10).
6. COQL `select COUNT(id) from Contacts where Giving_Last_Updated = '<yesterday>'` (about 1,600+ a day: 1,721 on 11/11, 1,617 on 12/11). Much lower = a nightly run failed: check Schedules > Failures.

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
