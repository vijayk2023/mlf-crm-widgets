# Zoho setup guide (quick start)

For anyone who has to set things up in Zoho CRM without a developer. Menu names below are from Zoho's current layout; if a label has moved, search for it in Setup (top search bar).
**Tip:** do each task once on a test Contact before telling the team.

You need: Zoho CRM **Administrator** access, and the files from this repo.

| What | Where it lives in this repo |
|---|---|
| Giving calculation and its wrappers (5 functions) | `deluge/` folder, see `deluge/README.md` |
| Lifetime Giving widget (zip to upload) | `widgets/lifetime_giving/dist/lifetime_giving.zip` |
| Income Report Export widget (zip to upload) | `widgets/income_stream/mlf-income-export-widget.zip` |

## Where everything is used in Zoho (the map)
Everything runs through **one calculation**, `mlf_aggregate_contact_giving`. Each place in Zoho that triggers it has its own small function that just calls it.

| Where in Zoho CRM | What it is | Function it calls | Repo file |
|---|---|---|---|
| **Contacts record > "Calculate Giving History" button** (Setup > Customization > Modules and Fields > Contacts > Buttons; page *In Record*, details page, Standard layout; Administrator and Fund Raising Team) | Staff click it to recalculate one contact | `mlf_calculate_giving_history` (category **Button**) | `deluge/mlf_calculate_giving_history.dg` |
| **Setup > Automation > Workflow Rules > "Auto-Update Giving Summary"** (module Donation, run on create or edit, all donations; action *Function*) | Recalculates the linked contact every time a donation is added or changed | `mlf_donation_trigger_aggregation` (category **Automation**) | `deluge/mlf_donation_trigger_aggregation.dg` |
| **Setup > Automation > Schedules** (five one-off schedules, to be created by hand; none exist yet) | Bulk refresh of all contacts. Run once, then switch off or delete | `mlf_refresh_batch_1..5` (category **Schedule**) -> `mlf_refresh_pages` (**Standalone**) | `deluge/mlf_refresh_batch_*.dg`, `deluge/mlf_refresh_pages.dg` |
| *(called by all of the above)* | The calculation itself | `mlf_aggregate_contact_giving` (category **Standalone**) | `deluge/mlf_aggregate_contact_giving.dg` |
| **Contacts record > left related list "ZohoMLF Lifetime Giving"** (Contacts Standard layout) | The widget. It only **reads** the giving fields; it calls no function | none (reads Contact fields) | `widgets/lifetime_giving/` |
| **Contacts layout > section "Life Time Value"** (Standard layout) | The 10 giving fields the calculation writes and the widget reads | none | n/a |

```
Button click ----------------> mlf_calculate_giving_history ----+
Donation created/edited -----> (workflow) mlf_donation_trigger_aggregation --+--> mlf_aggregate_contact_giving --> writes Contact fields --> widget shows them
Bulk run (optional) ---------> mlf_refresh_batch_N -> mlf_refresh_pages -----+
```

**To check what uses a function:** Setup > Developer Space > Functions > click the function > **View Associations**. A function with no *Associated* tag is not used by anything. Functions expected today: 9 (`mlf_aggregate_contact_giving`, `mlf_calculate_giving_history`, `mlf_donation_trigger_aggregation`, `mlf_refresh_pages`, and `mlf_refresh_batch_1` to `_5`). After the bulk run the five batch functions can be deleted (once their schedules are deleted).
**Before deleting a function,** remove or repoint whatever the Associations list shows, and see `docs/audit-2026-10-26.md` for the history.

---
## 1. Add the giving fields to Contacts (once)
Already done in the MicroLoan org (see `progress.md`). To check: **Setup > Customization > Modules and Fields > Contacts > Layouts > Standard**. You should see these fields under "Life Time Value": Lifetime Giving Total, Gift Aid Lifetime Total, Last Donation Amount/Date, Giving Year Breakdown, Giving Last Updated, **Giving YTD Total, Largest Donation Amount/Date, First Donation Date**.
If a field is missing, drag **New Field** onto the layout with the right type (Currency or Date) and the exact label. The API name must match (e.g. `Giving_YTD_Total`), or the function and widget will not find it.

## 2. Put the functions in Zoho (every time a .dg file changes)
The logic lives in one **standalone** function; buttons and schedules just call it. Files are in `deluge/` (see `deluge/README.md`).
1. **Setup > Developer Space > Functions**.
2. Open or create `mlf_aggregate_contact_giving` (category **Standalone**, arguments `id` String and `stampEmpty` Boolean). Paste the file, **Save**. **Line 1 must keep that function's name.**
3. Open or create `mlf_calculate_giving_history` (category **Button**, argument `id` String) and `mlf_refresh_pages` (Standalone, argument `pages` List). Paste, Save.
4. Test: **Execute** the shared function with a Contact ID (the long number in the record's URL) and `stampEmpty` true. The result should say `Updated: ... | Lifetime: £...`.

## 3. Add the "Calculate Giving History" button on Contacts (once)
1. **Setup > Customization > Modules and Fields > Contacts > Buttons > Create New Button**.
2. Name: `Calculate Giving History`. Define action: **Function**. Select Page: **In Record**, position **Details page**, layout **Standard**. (Only add an **In List** button if you want to run it for selected rows; the old Create/Clone and list buttons were removed on 2026-10-26.)
3. Configured Function: **Choose > Functions > MLF Calculate Giving History > Configure**, map argument `id` to **Contacts - Contact Id**, and give access to Administrator and Fund Raising Team. Any button you add later uses this same function.
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

## 6. Run the bulk refresh (five one-off schedules)
Use this when many contacts need the giving summary recalculated (first time, or each January so YTD resets). Each batch handles a block of Contacts pages (200 contacts per page). Batches skip contacts with no donations.

| Batch function | Contacts pages | Approx. contacts | Approx. run time |
|---|---|---|---|
| `mlf_refresh_batch_1` | 10-20 | 2,200 | 8-10 min |
| `mlf_refresh_batch_2` | 21-40 | 4,000 | 15-17 min |
| `mlf_refresh_batch_3` | 41-60 | 4,000 | 15-17 min |
| `mlf_refresh_batch_4` | 61-80 | 4,000 | 15-17 min |
| `mlf_refresh_batch_5` | 81-108 | 5,600 | 20-25 min |

(Pages 1-9 were already done on 2026-10-26. For a later full refresh, change batch 1 back to start at page 1 in `deluge/mlf_refresh_batch_1.dg` and the Zoho function.)

**For each batch** (Setup > Automation > Schedules > **Create New Schedule**; exact labels may differ slightly):
1. Name it after the batch, e.g. `Giving refresh batch 1`.
2. Frequency **Once**, with a start date and time. Start batch 1 now-ish and each later batch **at least 30 minutes after the previous one**, so runs never overlap.
3. Action: **Function**, choose `mlf_refresh_batch_N` (category Schedule).
4. Save and make sure the schedule shows as enabled.

**After each run:**
- Schedules page: check *Last Run* and the **Failures** tab.
- Setup > Developer Space > Functions > the batch function > **Logs**: the last line reads `Batch N complete. Updated: X | Skipped (no donations): Y`.
- Open two or three contacts from that page range and check the widget (see the test contacts in `sessions.md` for how to verify).

**When finished:** switch the five schedules off or delete them, then delete the batch functions if you do not want them. Rerunning a batch is safe (it just recalculates the same contacts).

---
## Troubleshooting
| Symptom | Likely cause / fix |
|---|---|
| Widget says "No giving data yet" | Not calculated yet. Click **Calculate Giving History** on that Contact. |
| Widget says "No donation received to date" | The Contact was calculated and has no gift above £0. Only trust it after the function is current (it is stamped with the check date). |
| New tiles missing or £0.00 | Function not updated in step 2, or the Contact has not been recalculated since. |
| "Page Not Found" opening a widget | Wrong index page path, or the zip has an extra top-level folder. The zip must contain `app/widget.html` at its top. |
| Year to date looks stale in January | Values change only when a contact is recalculated (button, a donation edit, or a bulk run, section 6). Run the bulk refresh in early January. |
| Function error mentioning a field | A field's API name differs from the code; check step 1. |

## Related docs
`sessions.md` (current state and next steps), `progress.md` (change log), `decisions.md` (why).
