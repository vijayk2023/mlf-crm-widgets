# Project Scope - crm-giving-widgets

## Purpose
Show each donor's giving history on the Contact (Lifetime Giving widget) from totals stored on Contact fields and kept current by Deluge functions.

## In scope
- Lifetime Giving widget (`widgets/lifetime_giving/`), Deluge functions (`deluge/`), Contact giving fields, the "Calculate Giving History" button, the donation-trigger workflow, bulk recalculation.

## Out of scope (unless agreed)
- Donation import/sync (see other projects); income_stream diagnostic widgets (untracked, undecided).

## Goals and status (as of 2026-11-10)
| Goal | Status |
|---|---|
| 1. Widget shows lifetime, YTD, Gift Aid, last/largest/first gift | Done, verified |
| 2. One shared calculation function + wrappers | Done (D12-D17) |
| 3. Live workflow keeps contacts current | Done |
| 4. Bulk recalculation of all Contacts | Done: pages 1-9 2026-10-26, batches 1-5 completed 27/10 (spot checks pending) |
| 6. Every Contact has a Lifetime Giving value (£0 for never-donors) for Zoho Campaigns segments (D19) | Backfill ran 10/11: 8,894 -> 93 blanks (one more editor run needed); on-create rule and refresh redeploy pending OK |
| 5. Edge cases (>200 donations, January YTD refresh) | Not started |

## Success criteria
- Every donor Contact has correct totals; spot checks match COQL.

## Open scope questions (see `WORKSPACE-PROGRESS.md` section 1)
- Fate of `widgets/income_stream/`; removing `cert.pem`/`key.pem` from git tracking.

## Stakeholders
Owner: Vijay Vasudevan. Users: MLF fundraising/donor care.
