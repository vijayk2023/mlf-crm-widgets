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
