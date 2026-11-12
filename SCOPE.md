# Project Scope - crm-giving-widgets

## Purpose
Show each donor's giving history on the Contact (Lifetime Giving widget) from totals stored on Contact fields and kept current by Deluge functions.

## In scope
- Lifetime Giving widget (`widgets/lifetime_giving/`), Deluge functions (`deluge/`), Contact giving fields, the "Calculate Giving History" button, the donation-trigger workflow, bulk recalculation.

## Out of scope (unless agreed)
- Donation import/sync (see other projects); income_stream diagnostic widgets (untracked, undecided).

## Goals and status (as of 2026-11-12)
| Goal | Status |
|---|---|
| 1. Widget shows lifetime, YTD, Gift Aid, last/largest/first gift | Done, verified |
| 2. One shared calculation function + wrappers | Done (D12-D17) |
| 3. Live workflow keeps contacts current | Done |
| 4. Bulk recalculation of all Contacts | Replaced by goal 7 (27/10 batches 2-5 had failed on the statement limit) |
| 6. Every Contact has a Lifetime Giving value (£0 for never-donors) for Zoho Campaigns segments (D19) | Done: 0 blanks since 10/11; kept at 0 by the nightly changed run (on-create rule optional) |
| 5. Edge cases (>200 donations, January YTD refresh) | >200 done (hardened function, up to 2,000); YTD fixed within ~2 weeks by goal 7 |
| 7. Nightly refresh keeps Last Donation Date / totals current for every Contact (D20) | Live from 11/11/2026; verify Failures (P13) |
| 8. Duplicate-contact report (review only) | Waiting for the user's Contacts export (P14) |

## Success criteria
- Every donor Contact has correct totals; spot checks match COQL.

## Open scope questions (see `WORKSPACE-PROGRESS.md` section 1)
- Fate of `widgets/income_stream/`; removing `cert.pem`/`key.pem` from git tracking.

## Stakeholders
Owner: Vijay Vasudevan. Users: MLF fundraising/donor care.
