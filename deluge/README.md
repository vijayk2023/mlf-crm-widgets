# Deluge functions

Source of truth for the Zoho CRM functions behind the giving widget. Edit the file here first, commit, then paste into Zoho
(Setup > Developer Space > Functions). **The first line must keep the Zoho function's own name** (see "Deploying").

## Structure (one calculation, thin wrappers)
| File | Type | Role |
|------|------|------|
| `mlf_aggregate_contact_giving.dg` | standalone | **All the logic.** Calculates and writes the giving fields for one Contact. `stampEmpty` = true writes even with no donations (button); false skips them (bulk). |
| `mlf_calculate_giving_history.dg` | button | One-line wrapper used by **all three** "Calculate Giving History" buttons. |
| `mlf_donation_trigger_aggregation.dg` | automation | Wrapper called by the workflow rule "Auto-Update Giving Summary" on every Donation create/edit. **Draft; compare with the code in Zoho first.** |
| `mlf_refresh_pages.dg` | standalone | Loops Contacts pages (200 per page) and calls the shared function. |
| `mlf_refresh_batch_1.dg` ... `_5.dg` | schedule | One-line wrappers: pages 1-20, 21-40, 41-60, 61-80, 81-108. Keep **off** unless a bulk run is wanted. |

Change the calculation in one file only. Retired copies (do not recreate): `mlf_lifetime_giving_aggregation`, `..._aggregation1`, `..._aggregation2`, `mlf_nightly_giving_refresh` (see decisions.md D12-D13).
The pre-refactor batch function is in git history at commit b1e0005.

## Deploying (in this order)
1. Create `mlf_aggregate_contact_giving` (standalone), test with **Execute** on a known contact.
2. Create `mlf_refresh_pages` (standalone). Test with pages `{1}` only.
3. Create `mlf_calculate_giving_history` (button). Repoint the buttons to it; test; then delete old functions.
4. Replace the code in `mlf_donation_trigger_aggregation` (the live workflow rule calls it).
5. Update the schedule wrappers, leave them off.

**Zoho syntax rules (D16):** signature on line 1 (comments go inside the body); argument types only `string,int,date,float,bool,map`; lists passed as comma-separated strings.
**Naming rule:** when pasting a repo file into a Zoho function, its first code line must carry that function's exact name, or the save may not take and old code keeps running. To confirm a paste worked, search the saved function for `received > 0` (shared function) or run the button on a contact with no donations: new code returns `Updated: ...`; the old copies returned `No donations found`.
**Calling syntax:** the wrappers call `standalone.mlf_aggregate_contact_giving(...)`. If Zoho's editor rejects it, use the form it suggests (e.g. `thisapp.standalone....`) and update the files.

## Commit labels
`<type>(<scope>): <summary>` - types `feat`, `fix`, `chore`, `docs`; scopes `deluge`, `widget`.
