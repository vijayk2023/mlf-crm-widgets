# WORKSPACE-PROGRESS.md - crm-giving-widgets

> **START OF EVERY SESSION:** follow the routine in the root `CLAUDE.md`. This file is the dashboard-level view; the detailed handoff is `sessions.md` (read it), change log `progress.md`, reasons `decisions.md`. **NOTE:** named WORKSPACE-PROGRESS.md because `progress.md` (the change log) already exists and Windows treats the names as the same file. **END:** update sections 1, 2, 3, add a section 5 entry, update `sessions.md`/`progress.md`/`decisions.md`, the `PROJECTS.md` row, commit in THIS repo (and the workspace repo for `PROJECTS.md`).

Last updated: 2026-11-10 (Session 3: read-only audit of the overnight £0 backfill; 93 blanks left).

---

## 1. PENDING (what is left)

### SUGGESTED NEXT (show this with the table below when the user says "resume the Lifetime Giving project")
1. **P8 finish (needs your OK) - recommended first:** run `mlf_fill_blank_giving` with `0` once from the Zoho function editor (one run = up to 800 Contacts) to fill the last 93 blanks; read the Logs/Failures of `mlf_fill_blank_giving_run` in Chrome; re-run the COQL blank count (target 0). Writes 93 Contacts; may fire any Contact on-edit workflows.
2. **P12 (you delete):** the 11 `mlf_fill_blank_giving_*` schedules and `mlf_fill_blank_giving_run` (keep `mlf_fill_blank_giving` for catch-up runs).
3. **P9 (needs your OK):** Contacts On Create rule, Lifetime Giving empty -> 0, so new Contacts never go blank again.
4. **P10 (needs your OK):** redeploy `mlf_refresh_pages` (stampEmpty true).
5. **P11 (you):** Zoho Campaigns segment count after the next CRM sync.
6. Then housekeeping: P2/P3 spot checks + old batch cleanup; decisions P4 (>200 donations) and P5 (January YTD); P6/P7 git.


| # | Item | Needs | Where to look |
|---|---|---|---|
| P1 | ~~Create 5 one-off schedules for batches 1-5~~ DONE: Schedules page shows all 5 Completed 27/10/2026 (found 2026-11-09) | Spot checks = P2 | Setup > Schedules |
| P8 | Verify the overnight £0 backfill. **Audited 2026-11-10:** blanks 8,894 -> **93**; £0 Contacts 1,013 -> 9,814 (+8,801). Shortfall is capacity, not errors: 11 runs x 800 = 8,800 < 8,894; the 93 left are the highest IDs (incl. 10 Contacts created 08/10/2026). Schedule Logs/Failures NOT yet read (needs Chrome) | **Your OK** to run `mlf_fill_blank_giving("0")` once from the editor, then read-only checks | `sessions.md` Next steps 1 |
| P9 | **NEW** Contacts workflow rule On Create: Lifetime Giving Total empty -> 0 | User OK, then I build and test | `sessions.md` Next steps 2 |
| P10 | **NEW** Redeploy `mlf_refresh_pages` (stampEmpty true, D19) | User OK | `deluge/mlf_refresh_pages.dg` |
| P11 | **NEW** Check Zoho Campaigns segment count after CRM sync | User | Zoho Campaigns |
| P12 | Delete the 11 `mlf_fill_blank_giving_*` schedules + `mlf_fill_blank_giving_run` once P8 reaches 0 blanks | User deletes | Setup > Schedules |
| P2 | Spot-check 3-4 contacts across page ranges after run; check function Logs/Failures | Read-only review | `sessions.md` |
| P3 | After clean run, switch off/delete schedules and batch functions | User deletes | `sessions.md` |
| P4 | Contacts with >200 donations understated (`searchRecords` first 200) | Decision | `sessions.md` Known issues |
| P5 | YTD stale each 1 January: annual bulk run? | Decision | `sessions.md` |
| P6 | Remove `cert.pem`/`key.pem` from git tracking; decide on untracked `widgets/income_stream/` | User OK | `sessions.md` |
| P7 | Check `git status -sb` for unpushed commits | User pushes | repo |

---

## 2. DECISIONS MADE

| When | Decision |
|---|---|
| 2026-10-26 | D1-D18 recorded in `decisions.md` (shared function + thin wrappers; never-donors skipped; £0/negative gifts ignored; user deletes in Zoho). |
| 2026-11-09 | D19: never-donors get Lifetime Giving £0 (supersedes D14); backfill via `mlf_fill_blank_giving` on 11 one-off schedules created by Claude at user request. |
| 2026-11-03 | Folder joined the workspace as `crm-giving-widgets`; stays its own git repo; parent ignores it. |

---

## 3. CURRENT STATE
Functions (11): `mlf_aggregate_contact_giving`, `mlf_calculate_giving_history`, `mlf_donation_trigger_aggregation`, `mlf_refresh_pages` (Zoho copy still stampEmpty false), `mlf_refresh_batch_1..5`, **`mlf_fill_blank_giving`** (standalone), **`mlf_fill_blank_giving_run`** (schedule). Button "Calculate Giving History" (Contacts). Workflow "Auto-Update Giving Summary" active. Schedules: `mlf_refresh_batch_1..5` completed 27/10 (off); `mlf_fill_blank_giving_01..11` enabled for 10/11/2026 01:00-06:00. Counts 2026-11-09: 21,491 Contacts, 8,894 blank Lifetime Giving, 1,013 at 0. **Counts 2026-11-10 (after the overnight run): 21,485 Contacts, 93 blank, 9,814 at £0.** All 11 schedules were due to have run; their logs not yet read.

### Quick health checks (read-only)
1. `getRecord` Jillian Atkins (baseline 2026-11-10): lifetime £2,820, YTD £550, largest £500 (2026-06-25), first 2007-12-19, last £5 (2026-10-02), Giving_Last_Updated 2026-10-29. Changes only when she gives again.
2. James Wilson (baseline 2026-11-10): £11.99 lifetime/YTD/largest/last, first and last gift 2026-10-08 (no longer a never-donor). Never-donor test: Titus Gwemende 901783000003919050 (£0).
3. `git status -sb` in this folder.
4. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total is null` (93 on 2026-11-10; target 0 after the P8 finishing run).
5. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total = 0` (9,814 on 2026-11-10).

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
