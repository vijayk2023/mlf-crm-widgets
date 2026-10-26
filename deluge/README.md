# Deluge functions

Source of truth for Zoho CRM functions used by the widgets. Paste the file contents into
Setup > Developer Hub > Functions; update the file here first, then deploy.

| File | Trigger | Writes to |
|------|---------|-----------|
| `mlf_lifetime_giving_aggregation_all.dg` | "Calculate Giving History" button (Contacts) | Giving fields on Contacts, read by `widgets/lifetime_giving` |
| `mlf_refresh_batch_1.dg` | Scheduled function, contacts pages 1-20 (4,000 contacts). **Baseline of the old logic - not yet updated** | Same Contact giving fields |

## Deploying: one rule that bites
Zoho has more than one copy of this function (e.g. `mlf_lifetime_giving_aggregation_all` and `mlf_lifetime_giving_aggregation_all1`), one per button. Each copy's **first line must keep that copy's own name**. When pasting the repo file into a copy, change line 1 to that function's name, otherwise the save may not take and the old code keeps running. To confirm a paste worked, search the saved function for `received > 0`, or run the button on a contact with no donations: the new code returns `Updated: ...`, the old code returns `No donations found`.

## Commit labels
`<type>(<scope>): <summary>` - types `feat`, `fix`, `chore`, `docs`; scopes `deluge`, `widget`.
