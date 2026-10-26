# Sessions

Read this first. "Current state" is kept up to date; each session below it records what was done and what to do next. At the end of every session, update Current state and add a "Next session" line.

## Current state (as of 2026-10-26)
- Repo: pushed to `origin/main` on 2026-10-26 (in sync, head ffe370e). Untracked: `widgets/income_stream/` (diagnostic and export zips) - not part of this repo yet.
- Widget code and `dist/lifetime_giving.zip`: uploaded to Zoho; display verified in the browser on a populated contact. Calculation verified on a multi-gift contact (ties, multiple years).
- Deluge `deluge/mlf_lifetime_giving_aggregation_all.dg` (D9-D11 version) and the new `dist/lifetime_giving.zip` are deployed in Zoho. **Jillian Atkins retest passed** (user-confirmed 2026-10-26). No-donation contact test and bulk recalculation still outstanding.
- CRM: 4 new Contact fields exist and are on the layout (Life Time Value). Two test contacts recalculated (one only after the fix). Contacts has 3 buttons: Calculate Giving History (Create/Clone, no layout - likely leftover), Calculate Giving History (Details, Standard), Calculate Giving History All (list mass action).

## Known issues / not done
- `searchRecords(...,1,200)` reads only the first 200 donations per contact; larger donors get understated totals.
- Fixed 2026-10-26 (D11): no-donation contacts are now cleared and stamped.
- YTD goes stale on 1 Jan until each contact is recalculated; consider a scheduled bulk run.
- `cert.pem` and `key.pem` (local dev HTTPS) are committed in `widgets/lifetime_giving/`; remove from tracking and gitignore.
- `widgets/income_stream/` is untracked.

## Session 2026-10-26 - Giving widget enhancements
Done: found widget (reader of Contact fields); confirmed GitHub matched local; created 4 Contact fields; imported live Deluge and added YTD/largest/first; redesigned widget grid; repacked dist; wrote these docs and the Zoho setup guide (docs/zoho-setup-guide.md).
Left: deploy and verify (below), push.
**Next session:** test a contact with no donations (should read "No donation received to date", and an uncalculated one should still say "No giving data yet") -> bulk-recalculate every Contact with donations (list view mass action) -> spot-check a few, then consider 200-record paging and a scheduled January refresh. Then pick from Known issues (200-record paging first).
