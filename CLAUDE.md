# Project: crm-giving-widgets   (MLF CRM widgets)

**Apps:** CRM  (load `shared/zoho-crm.md`)
**Status:** see `../../PROJECTS.md`. Files: `WORKSPACE-PROGRESS.md` (dashboard-level pending/decisions/state/log; NOT `PROGRESS.md`, which is the same file as the change log `progress.md` on Windows), `SCOPE.md`, plus this repo's own detailed logs `sessions.md` (START HERE), `progress.md` (change log), `decisions.md` (D1-D20).
**Nested repo:** this folder is its own git repo (`vijayk2023/mlf-crm-widgets`, branch `main`). The parent workspace repo ignores it. Commit docs/code HERE; push only when the user asks (`! git push origin main` if denied).

Lifetime Giving widget on Contacts plus the Deluge functions that calculate giving totals.

**Resume rule (user request, 2026-11-10):** when the user says "resume the Lifetime Giving project" (or similar), after the dashboard and org guard: show the **Lifetime Giving: pending items** table from `WORKSPACE-PROGRESS.md` section 1 (status + what each needs), run the section 3 health checks and report drift against the baselines, then show the **SUGGESTED NEXT** list with a recommended first pick and ask which to take. Refresh that block at every session end.

**Start every session by reading `sessions.md`** - the "START HERE" block has the model/effort used last time, tools, rules, test records and the next steps. Then `decisions.md` if you need the why, `docs/zoho-setup-guide.md` for how Zoho is wired, `deluge/README.md` for the functions.

At the end of a session: update `sessions.md` (Current state, Next steps, session log), `progress.md` (change log) and `decisions.md` (new decisions only), then the workspace-level `WORKSPACE-PROGRESS.md`, `SCOPE.md` and `../../PROJECTS.md` row. Commit locally with labels `type(scope): summary`.

Hard rules: never delete anything in Zoho (the user deletes); Deluge files in `deluge/` are the source of truth (edit file, commit, then paste into Zoho; line 1 of a function must be its signature); do not work around a blocked tool or permission.

## PEER DECISIONS 24 Nov 2026: review before working on this project
Source: `Downloads\Critical list to do.xlsx`, column D (agreed with the peers); full context in `../crm-requirements-assessment/ASSESSMENT-v5-combined.md` sections 3.6 and 3.12. Check each point against `PROGRESS.md` section 1 before starting; flag any conflict to the user. Delete this block once every point is reflected in `PROGRESS.md` and acted on.
- **#12 decided:** Accounts get a giving history with the same data/date fields as Contacts (about 3,587 Donations have an Account) (P15). If a soft credit points to a Contact or Account that does not exist yet, create it first, e.g. Visa Account `901783000002330577` (P16).
- **#6 decided:** giving history separates direct donations from soft credits, with a total for each (P18). An amount field lets only part of a gift be soft credited (e.g. GBP 10 of GBP 20); the peer accepted this is the harder part and will be explored during the build (P17). Benevity matched gifts start on the company Account and are soft credited to the employee (P19).
- **Still open (requirements Q6):** one or several soft credits per gift; backfill existing soft credits at 100%; whether soft credits count in Lifetime Giving. Field names are needed in step 1 for the templates (P17 design only).
- **Order:** the 230 organisation Contacts move to Accounts first (`crm-accounts-picklists` A5), or their gifts are counted on Contacts. Changing the shared giving function needs the user's explicit approval. Proposed dates: step 3 = 11-13 Jan, step 4 = 13-18 Jan (awaiting the user's OK).

## Project-specific facts
- Test contacts: Jillian Atkins 901783000002331815, James Wilson 901783000003579001, Gillian Eatwell 901783000002352993 (expected values in `sessions.md`).
- Contacts "Life Time Value" fields: giving totals plus `Giving_YTD_Total`, `Largest_Donation_Amount/Date`, `First_Donation_Date`.
- Workflow "Auto-Update Giving Summary" (Donation create/edit) is LIVE and calls `mlf_donation_trigger_aggregation`; do not change without approval.
