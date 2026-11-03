# WORKSPACE-PROGRESS.md - crm-giving-widgets

> **START OF EVERY SESSION:** follow the routine in the root `CLAUDE.md`. This file is the dashboard-level view; the detailed handoff is `sessions.md` (read it), change log `progress.md`, reasons `decisions.md`. **NOTE:** named WORKSPACE-PROGRESS.md because `progress.md` (the change log) already exists and Windows treats the names as the same file. **END:** update sections 1, 2, 3, add a section 5 entry, update `sessions.md`/`progress.md`/`decisions.md`, the `PROJECTS.md` row, commit in THIS repo (and the workspace repo for `PROJECTS.md`).

Last updated: imported into workspace 2026-11-03 (content as of 2026-10-26).

---

## 1. PENDING (what is left)

| # | Item | Needs | Where to look |
|---|---|---|---|
| P1 | Create 5 one-off schedules for `mlf_refresh_batch_1..5` (pages 10-108), run, verify | User creates schedules; then read-only check | `sessions.md` Next steps; `docs/zoho-setup-guide.md` s6 |
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
| 2026-11-03 | Folder joined the workspace as `crm-giving-widgets`; stays its own git repo; parent ignores it. |

---

## 3. CURRENT STATE
Functions (9): `mlf_aggregate_contact_giving`, `mlf_calculate_giving_history`, `mlf_donation_trigger_aggregation`, `mlf_refresh_pages`, `mlf_refresh_batch_1..5`. Button "Calculate Giving History" (Contacts). Workflow "Auto-Update Giving Summary" active. No schedules exist. 4 custom Contact fields added.

### Quick health checks (read-only)
1. `getRecord` Jillian Atkins: lifetime £2,300, YTD £30, largest £500 (2025-12-18), first 2007-12-19.
2. James Wilson: £0, blank dates.
3. `git status -sb` in this folder.

---

## 4. PROJECT-SPECIFIC LIMITS AND GOTCHAS
See `sessions.md` (Zoho function syntax rules, D16).

---

## 5. SESSION LOG

### Session 0 (2026-10-26, Sonnet 5.5) - before import to workspace
Full log in `sessions.md`. Widget enhanced, shared function refactor, pages 1-9 recalculated.

### Session 1 (2026-11-03) - Imported into workspace
Moved from `Zoho MLF Widget Projects/mlf-crm-widgets`; added CLAUDE/SCOPE/PROGRESS; nothing run in Zoho.
