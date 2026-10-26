# Progress / Change log

Newest first. One entry per meaningful change; commit labels follow `type(scope): summary`
(types `feat` `fix` `chore` `docs`; scopes `deluge` `widget` `crm` `docs`).
Per-session status and next steps live in [sessions.md](sessions.md); the reasoning behind changes is in [decisions.md](decisions.md).

## 2026-10-26
- `test`: verified on Jillian Atkins (178 gifts, 2007-2026): YTD £30, largest £500 on 2025-12-18 (tie with 2022-07-12 resolved to the later), first gift 2007-12-19. User confirmed it worked. Widget display also verified in the browser on Gillian Eatwell.
- `crm`: replaced the function behind the Contacts button **Calculate Giving History** (Record - Details, Standard) with the repo version; previous code not kept. Verified on Gillian Eatwell: YTD, largest and first-gift fields populated correctly (single £50 gift, 2026-02-23).
- `feat(deluge)`: return message now includes YTD, largest and first gift so a run shows at once whether the new code is live.
- `docs`: added progress.md, decisions.md, sessions.md, CLAUDE.md, docs/zoho-setup-guide.md.
- `chore(widget)` 008c545: repacked `widgets/lifetime_giving/dist/lifetime_giving.zip` from `app/` (was the 17 Sep build).
- `feat(widget)` b62c92f: Lifetime Giving widget shows YTD, Gift Aid, Last gift, Largest gift (amount + date) and First gift in a 2x2 grid; `renderWidget` now takes one object. Tiles for largest/first/last hide when their date is empty.
- `feat(deluge)` 3449d62: `mlf_lifetime_giving_aggregation_all` now writes `Giving_YTD_Total`, `Largest_Donation_Amount`, `Largest_Donation_Date`, `First_Donation_Date`; removed the duplicated `updateRecord` call.
- `chore(deluge)` edcd0e0: imported the live function verbatim as a baseline (`deluge/`).
- `crm`: created 4 custom fields on Contacts (currency x2, date x2) in the MicroLoan org, then moved to the Life Time Value section. Not in git (CRM config).
  IDs: YTD 901783000003879001, Largest amount ...009, Largest date ...017, First date ...025.

## Before 2026-10-26
- 11f1df6 fix: add ZohoCRM.min.js auth layer and correct SDK version to 1.0.5
- 03ed696 feat: switch widget to URL params (removes SDK dependency)
- 3f66214 chore: remove node_modules from tracking
- 5d1b737 chore: add root index.html to activate GitHub Pages
- 2bebe5d feat: lifetime-giving widget
