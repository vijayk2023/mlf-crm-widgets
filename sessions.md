# Sessions

Read this first. "Current state" is kept up to date; each session below it records what was done and what to do next. At the end of every session, update Current state and add a "Next session" line.

## Current state (as of 2026-10-26)
- Repo: local `main` is 9 commits ahead of `origin/main` (11f1df6); **not pushed**. Untracked: `widgets/income_stream/` (diagnostic and export zips).
- Widget code and `dist/lifetime_giving.zip`: uploaded to Zoho; display verified in the browser on a populated contact. Calculation verified on a multi-gift contact (ties, multiple years).
- Deluge `deluge/mlf_lifetime_giving_aggregation_all.dg`: **live behind the Record - Details button** and verified on one contact. The deployed copy has the older return message (no YTD/largest/first); repaste the latest. The list-view **Calculate Giving History All** button's function is unchecked.
- CRM: 4 new Contact fields exist and are on the layout (Life Time Value). Two test contacts recalculated (one only after the fix). Contacts has 3 buttons: Calculate Giving History (Create/Clone, no layout - likely leftover), Calculate Giving History (Details, Standard), Calculate Giving History All (list mass action).

## Known issues / not done
- `searchRecords(...,1,200)` reads only the first 200 donations per contact; larger donors get understated totals.
- No-donation contacts return early, leaving stale values (including YTD) on the record.
- YTD goes stale on 1 Jan until each contact is recalculated; consider a scheduled bulk run.
- `cert.pem` and `key.pem` (local dev HTTPS) are committed in `widgets/lifetime_giving/`; remove from tracking and gitignore.
- `widgets/income_stream/` is untracked.

## Session 2026-10-26 - Giving widget enhancements
Done: found widget (reader of Contact fields); confirmed GitHub matched local; created 4 Contact fields; imported live Deluge and added YTD/largest/first; redesigned widget grid; repacked dist; wrote these docs and the Zoho setup guide (docs/zoho-setup-guide.md).
Left: deploy and verify (below), push.
**Next session:** repaste latest `.dg` (return message) -> check which function the "All" button calls and point it at the same code -> decide whether £0 gifts should count as the first gift -> bulk-recalculate and `git push`. Then pick from Known issues (200-record paging first).
