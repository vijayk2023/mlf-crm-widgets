# Sessions

Read this first. "Current state" is kept up to date; each session below it records what was done and what to do next. At the end of every session, update Current state and add a "Next session" line.

## Current state (as of 2026-10-26)
- Repo: local `main` is 5 commits ahead of `origin/main` (11f1df6); **not pushed**. Untracked: `widgets/income_stream/` (diagnostic and export zips).
- Widget code and `dist/lifetime_giving.zip`: done, **not yet uploaded to Zoho** or tested in a browser.
- Deluge `deluge/mlf_lifetime_giving_aggregation_all.dg`: done in repo, **not yet pasted into Zoho**, untested.
- CRM: 4 new Contact fields exist and are on the layout (Life Time Value). No contact has been recalculated.

## Known issues / not done
- `searchRecords(...,1,200)` reads only the first 200 donations per contact; larger donors get understated totals.
- No-donation contacts return early, leaving stale values (including YTD) on the record.
- YTD goes stale on 1 Jan until each contact is recalculated; consider a scheduled bulk run.
- `cert.pem` and `key.pem` (local dev HTTPS) are committed in `widgets/lifetime_giving/`; remove from tracking and gitignore.
- `widgets/income_stream/` is untracked.

## Session 2026-10-26 - Giving widget enhancements
Done: found widget (reader of Contact fields); confirmed GitHub matched local; created 4 Contact fields; imported live Deluge and added YTD/largest/first; redesigned widget grid; repacked dist; wrote these docs.
Left: deploy and verify (below), push.
**Next session:** paste the updated `.dg` into the Zoho function -> upload `dist/lifetime_giving.zip` in widget settings -> run Calculate Giving History on one test contact and compare the 5 values with its Donation records -> if good, bulk-recalculate and `git push`. Then pick from Known issues (200-record paging first).
