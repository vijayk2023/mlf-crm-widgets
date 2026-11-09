# WORKSPACE-PROGRESS.md - crm-giving-widgets

> **START OF EVERY SESSION:** follow the routine in the root `CLAUDE.md`. This file is the dashboard-level view; the detailed handoff is `sessions.md` (read it), change log `progress.md`, reasons `decisions.md`. **NOTE:** named WORKSPACE-PROGRESS.md because `progress.md` (the change log) already exists and Windows treats the names as the same file. **END:** update sections 1, 2, 3, add a section 5 entry, update `sessions.md`/`progress.md`/`decisions.md`, the `PROJECTS.md` row, commit in THIS repo (and the workspace repo for `PROJECTS.md`).

Last updated: 2026-11-09 (Session 2: Lifetime Giving £0 backfill scheduled).

---

## 1. PENDING (what is left)

| # | Item | Needs | Where to look |
|---|---|---|---|
| P1 | ~~Create 5 one-off schedules for batches 1-5~~ DONE: Schedules page shows all 5 Completed 27/10/2026 (found 2026-11-09) | Spot checks = P2 | Setup > Schedules |
| P8 | **NEW** Verify the overnight £0 backfill (11 schedules, 10/11 01:00-06:00): blank count 8,894 -> 0, logs, spot checks | Read-only review | `sessions.md` Next steps 1 |
| P9 | **NEW** Contacts workflow rule On Create: Lifetime Giving Total empty -> 0 | User OK, then I build and test | `sessions.md` Next steps 2 |
| P10 | **NEW** Redeploy `mlf_refresh_pages` (stampEmpty true, D19) | User OK | `deluge/mlf_refresh_pages.dg` |
| P11 | **NEW** Check Zoho Campaigns segment count after CRM sync | User | Zoho Campaigns |
| P12 | **NEW** Delete the 11 `mlf_fill_blank_giving_*` schedules + `mlf_fill_blank_giving_run` after P8 | User deletes | Setup > Schedules |
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
Functions (11): `mlf_aggregate_contact_giving`, `mlf_calculate_giving_history`, `mlf_donation_trigger_aggregation`, `mlf_refresh_pages` (Zoho copy still stampEmpty false), `mlf_refresh_batch_1..5`, **`mlf_fill_blank_giving`** (standalone), **`mlf_fill_blank_giving_run`** (schedule). Button "Calculate Giving History" (Contacts). Workflow "Auto-Update Giving Summary" active. Schedules: `mlf_refresh_batch_1..5` completed 27/10 (off); `mlf_fill_blank_giving_01..11` enabled for 10/11/2026 01:00-06:00. Counts 2026-11-09: 21,491 Contacts, 8,894 blank Lifetime Giving, 1,013 at 0.

### Quick health checks (read-only)
1. `getRecord` Jillian Atkins: lifetime £2,300, YTD £30, largest £500 (2025-12-18), first 2007-12-19.
2. James Wilson: £0, blank dates.
3. `git status -sb` in this folder.
4. COQL `select COUNT(id) from Contacts where Lifetime_Giving_Total is null` (target 0 after 10/11).

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
