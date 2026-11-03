# Project: crm-giving-widgets   (MLF CRM widgets)

**Apps:** CRM  (load `shared/zoho-crm.md`)
**Status:** see `../../PROJECTS.md`. Files: `WORKSPACE-PROGRESS.md` (dashboard-level pending/decisions/state/log; NOT `PROGRESS.md`, which is the same file as the change log `progress.md` on Windows), `SCOPE.md`, plus this repo's own detailed logs `sessions.md` (START HERE), `progress.md` (change log), `decisions.md` (D1-D18).
**Nested repo:** this folder is its own git repo (`vijayk2023/mlf-crm-widgets`, branch `main`). The parent workspace repo ignores it. Commit docs/code HERE; push only when the user asks (`! git push origin main` if denied).

Lifetime Giving widget on Contacts plus the Deluge functions that calculate giving totals.

**Start every session by reading `sessions.md`** - the "START HERE" block has the model/effort used last time, tools, rules, test records and the next steps. Then `decisions.md` if you need the why, `docs/zoho-setup-guide.md` for how Zoho is wired, `deluge/README.md` for the functions.

At the end of a session: update `sessions.md` (Current state, Next steps, session log), `progress.md` (change log) and `decisions.md` (new decisions only), then the workspace-level `WORKSPACE-PROGRESS.md`, `SCOPE.md` and `../../PROJECTS.md` row. Commit locally with labels `type(scope): summary`.

Hard rules: never delete anything in Zoho (the user deletes); Deluge files in `deluge/` are the source of truth (edit file, commit, then paste into Zoho; line 1 of a function must be its signature); do not work around a blocked tool or permission.

## Project-specific facts
- Test contacts: Jillian Atkins 901783000002331815, James Wilson 901783000003579001, Gillian Eatwell 901783000002352993 (expected values in `sessions.md`).
- Contacts "Life Time Value" fields: giving totals plus `Giving_YTD_Total`, `Largest_Donation_Amount/Date`, `First_Donation_Date`.
- Workflow "Auto-Update Giving Summary" (Donation create/edit) is LIVE and calls `mlf_donation_trigger_aggregation`; do not change without approval.
