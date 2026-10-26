# Deluge functions

Source of truth for Zoho CRM functions used by the widgets. Paste the file contents into
Setup > Developer Hub > Functions; update the file here first, then deploy.

| File | Trigger | Writes to |
|------|---------|-----------|
| `mlf_lifetime_giving_aggregation_all.dg` | "Calculate Giving History" button (Contacts) | Giving fields on Contacts, read by `widgets/lifetime_giving` |

## Commit labels
`<type>(<scope>): <summary>` - types `feat`, `fix`, `chore`, `docs`; scopes `deluge`, `widget`.
