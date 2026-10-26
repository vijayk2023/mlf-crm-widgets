# Decisions

Architecture/design decisions, newest last. Format: context, decision, consequences. Add a new entry each session; never rewrite old ones - supersede them.

## D1 - Widget stays a pure reader; calculation is extended (2026-10-26)
Context: stakeholders want YTD, last/largest/first gift and lifetime total on the Contact. Options were extend the Deluge calculation, fields already exist, or compute in the widget.
Decision: extend the "Calculate Giving History" Deluge function and store results on Contact fields; the widget only reads them.
Consequences: widget is fast and simple; values are only as fresh as the last run of the button (see sessions.md known issues).

## D2 - YTD is the calendar year (2026-10-26)
1 Jan to today, compared on the year of `Donation_Date` against the current year. Financial-year basis was offered and declined.

## D3 - Largest gift ties go to the most recent gift (2026-10-26)
Equal amounts: the later `Donation_Date` wins. Chosen so "largest gift" shows the most current example.

## D4 - Four new Contact fields, API names fixed (2026-10-26)
`Giving_YTD_Total`, `Largest_Donation_Amount` (currency), `Largest_Donation_Date`, `First_Donation_Date` (date). Created through the Zoho CRM MCP connection in the MicroLoan org. The widget and Deluge both depend on these exact API names.

## D5 - Deluge source is version-controlled in `deluge/` (2026-10-26)
Context: the function lived only in the CRM, with several versions; nothing in git.
Decision: git is the source of truth. Workflow: edit the `.dg` file, commit, then paste into Zoho. The live function was imported verbatim as a baseline commit before any change so diffs are reviewable.

## D6 - Commit labelling convention (2026-10-26)
`type(scope): summary` as documented in progress.md and deluge/README.md.

## D7 - Widget render API takes one object (2026-10-26)
`renderWidget(d)` replaces six positional arguments, since the field count grew. Stat tiles are a CSS grid (2 columns), reusing existing `.wg-stat` styles.

## D8 - Push only after the Deluge is deployed and verified (2026-10-26)
The repo has GitHub Pages enabled, and the widget would show YTD as £0.00 until contacts are recalculated. Local commits first; push after a test contact is verified.

## D9 - Only gifts above £0 count for first, last and largest (2026-10-26)
Context: £0.00 Donation records exist (pledges/import placeholders, e.g. a 2012 record on Jillian Atkins). They could become the "first gift" date or a "last gift" of £0.00. Negative amounts (reversals/refunds, many from 2017-2018) also exist.
Decision: first gift, last gift and largest gift consider only `Received_Amount > 0`. Lifetime, YTD and the by-year breakdown stay **net** (negatives included); years whose records are all £0 are left out of the breakdown. Gift Aid total unchanged.
Consequences: `Last_Donation_Date/Amount` can differ from before for contacts whose newest record was £0 (treated as a fix). A contact with only reversals shows a net total but no first/last/largest.

## D10 - Distinguish "not calculated" from "no donation received" (2026-10-26)
Context: the widget showed one empty state ("No giving data yet - click Calculate") for everything. Saying "no donation" for an uncalculated contact would be false for thousands of contacts until the bulk run.
Decision: three widget states, driven by existing fields, no new field. No `Giving_Last_Updated` and nothing else: "No giving data yet - click Calculate". `Giving_Last_Updated` set, lifetime <= 0 and no First_Donation_Date: "No donation received to date". Otherwise the summary. The `First_Donation_Date` test stops contacts calculated by the old function (stamped, positive lifetime, no first date) being mislabelled.
Consequences: the message is only as trustworthy as the last calculation; recalculation is the source of truth.

## D11 - The calculation always writes, even with no donations (2026-10-26)
Context: with no Donation records the function returned early without writing, so stale totals stayed (e.g. a donation moved to another contact) and the contact could never be stamped as checked.
Decision: remove the early return; with no donations the function writes zero totals, blank dates and stamps `Giving_Last_Updated`. Required by D10.

## D12 - One shared calculation function; buttons and batches are wrappers (2026-10-26)
Context: the same logic existed in at least `mlf_lifetime_giving_aggregation_all`, `..._all1`, `mlf_nightly_giving_refresh` and `mlf_refresh_batch_1..5`. A repaste to one copy silently failed and an old copy kept running (James Wilson test).
Decision: a standalone function `mlf_aggregate_contact_giving(id, stampEmpty)` holds all logic; one button function `mlf_calculate_giving_history` serves all three buttons; `mlf_refresh_pages(pages)` holds the batch loop; `mlf_refresh_batch_1..5` are one-line schedule wrappers. Old functions are deleted after the buttons are repointed and verified.
Consequences: logic changes happen in one file. Depends on a button/schedule function being able to call a standalone function (`standalone.fn(...)`); verified at deploy time. Repo files now carry the same name Zoho shows.

## D13 - `mlf_nightly_giving_refresh` is retired (2026-10-26)
It was pages 1-108 in one run, i.e. batches 1-5 combined; the batches exist because one run is too long. All schedule functions were switched off by the user before this change; none should be enabled until tested.

## D14 - Bulk refresh skips contacts with no donations (default), button stamps them (2026-10-26)
Context: stamping every Contact (up to ~21,600, one update each) costs API calls; the old batches also skipped them.
Decision: `stampEmpty` is false in batches, true in the button. Never-donors then show "No giving data yet - click Calculate" until someone clicks, then "No donation received to date". Trade-off accepted pending user review; flip the `false` in `mlf_refresh_pages.dg` to stamp everyone.
Consequences: stale totals on a contact whose donations were all removed persist until the button is clicked.

## D15 - The Donation workflow trigger also goes through the shared function (2026-10-26)
Context: the audit (docs/audit-2026-10-26.md) found an active workflow rule "Auto-Update Giving Summary" calling `mlf_donation_trigger_aggregation` on every Donation create/edit, running the old logic.
Decision: that function becomes a wrapper that looks up the donation's Contact and calls `mlf_aggregate_contact_giving(contactId, true)`. The workflow rule is unchanged.
Consequences: giving fields stay current (including YTD, largest, first) without manual runs; every donation edit costs the shared function's API calls.

## D16 - Zoho function syntax rules learned at deploy (2026-10-26)
Found when deploying (editor validation errors): the signature must be on **line 1** (header comments go inside the body); argument types are only `string,int,date,float,bool,map` (so `bool` not `Boolean`, and a list is passed as a comma-separated `String` and split with `.toList(",")`); `standalone.mlf_aggregate_contact_giving(...)` is the working call syntax from button and automation functions. Repo files were corrected to match. The earlier "paste didn't stick" problem was a naming/format issue of this kind.

## D17 - Cleanup executed: one button, one workflow trigger, no standing schedules (2026-10-26)
Context: after D12-D16 the old copies were still in Zoho (3 Contacts buttons, 6 schedules, 3 old button functions, the nightly function).
Decision, carried out by the user and re-audited by Claude (read-only): Contacts has a single button "Calculate Giving History" (In Record, details page) calling `mlf_calculate_giving_history`; the three old buttons, the nightly and batch schedules and the old functions were deleted; the workflow rule "Auto-Update Giving Summary" stays as the freshness mechanism for new/edited donations; no schedule runs by default. Bulk refresh is run on demand by recreating the batch schedule functions from `deluge/`.
Consequences: no list-view mass-action button (not recreated); existing contacts only gain the new fields when recalculated (button, donation edit, or an on-demand bulk run); YTD needs a yearly bulk run. Function map and where each is used: docs/zoho-setup-guide.md ("Where everything is used").
Claude never deletes Zoho items; deletions are done by the user.
