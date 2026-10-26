# Zoho setup guide (quick start)

For anyone who has to set things up in Zoho CRM without a developer. Menu names below are from Zoho's current layout; if a label has moved, search for it in Setup (top search bar).
**Tip:** do each task once on a test Contact before telling the team.

You need: Zoho CRM **Administrator** access, and the files from this repo.

| What | Where it lives in this repo |
|---|---|
| Calculate Giving History function | `deluge/mlf_lifetime_giving_aggregation_all.dg` |
| Lifetime Giving widget (zip to upload) | `widgets/lifetime_giving/dist/lifetime_giving.zip` |
| Income Report Export widget (zip to upload) | `widgets/income_stream/mlf-income-export-widget.zip` |

---
## 1. Add the giving fields to Contacts (once)
Already done in the MicroLoan org (see `progress.md`). To check: **Setup > Customization > Modules and Fields > Contacts > Layouts > Standard**. You should see these fields under "Life Time Value": Lifetime Giving Total, Gift Aid Lifetime Total, Last Donation Amount/Date, Giving Year Breakdown, Giving Last Updated, **Giving YTD Total, Largest Donation Amount/Date, First Donation Date**.
If a field is missing, drag **New Field** onto the layout with the right type (Currency or Date) and the exact label. The API name must match (e.g. `Giving_YTD_Total`), or the function and widget will not find it.

## 2. Put the function in Zoho (every time the .dg file changes)
1. **Setup > Developer Space > Functions** (older menus: *Automation > Actions > Functions*).
2. Open the existing function **mlf_lifetime_giving_aggregation_all**. (First-time setup: **New Function**, category **Button**, name `mlf_lifetime_giving_aggregation_all`, add one argument `id` of type String.)
3. Replace the whole code with the contents of the `.dg` file from the repo, then **Save**.
4. Optional test: use **Execute** with a Contact's ID (the long number in the record's URL). The result line should say `Updated: ... | Lifetime: £...`.

## 3. Add the "Calculate Giving History" button on Contacts (once)
1. **Setup > Customization > Modules and Fields > Contacts > Links and Buttons > Create New Button**.
2. Name: `Calculate Giving History`. Place it in **Details page** (add **List view** too only if you want to run it for selected rows).
3. Action: **Writing function**, pick `mlf_lifetime_giving_aggregation_all`, and map argument `id` to the Contact's **Contact Id**.
4. Save, then make sure your profiles can see the button (Setup > Users and Control > Profiles > the Contacts permissions).
5. **Use it:** open a Contact > click **Calculate Giving History**. Refresh the page; the widget shows the new numbers.

## 4. Install or update the Lifetime Giving widget
1. **Setup > Developer Space > Widgets**.
2. Update: open **Lifetime Giving** > **Edit**, upload the zip (Hosting: **Zoho**), **Index page `/app/widget.html`**, Save. First time: **Create New Widget**, type **Detail Page**, same settings.
3. Show it on the Contact page: edit the Contacts layout (**Setup > Customization > Modules and Fields > Contacts > Layouts > Standard**) or the Canvas detail view, add the widget as a section/component, and Save.
4. Open a Contact that has donations. If it says "No giving data yet", click the button first.

## 5. Install or update the Income Report Export widget
1. **Setup > Developer Space > Widgets > Create New Widget** (or edit the existing one).
2. Name: `MLF Income Report Export`; Hosting **Zoho**; upload `mlf-income-export-widget.zip`; Index page `/app/widget.html`.
3. Type: pick the placement your team uses (a Web Tab or a button). **Confirm which was used originally; this is not recorded in the repo.**
4. Open it, choose a donation date range (This Week, Last 7 Days, Last Week or Custom) and click **Export Clean XLSX**. It downloads an Excel file with the Donations and their split transactions.

## Troubleshooting
| Symptom | Likely cause / fix |
|---|---|
| Widget says "No giving data yet" | Click **Calculate Giving History** on that Contact. |
| Button says "No donations found" | The Contact has no records in the Donation module linked to it. |
| New tiles missing or £0.00 | Function not updated in step 2, or the Contact has not been recalculated since. |
| "Page Not Found" opening a widget | Wrong index page path, or the zip has an extra top-level folder. The zip must contain `app/widget.html` at its top. |
| Year to date looks stale in January | Values only change when the button is clicked. Recalculate. |
| Function error mentioning a field | A field's API name differs from the code; check step 1. |

## Related docs
`sessions.md` (current state and next steps), `progress.md` (change log), `decisions.md` (why).
