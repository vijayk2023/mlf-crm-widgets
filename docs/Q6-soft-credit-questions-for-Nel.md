Subject: Soft credits - 3 decisions needed (Q6), with worked examples

[Anonymised copy, 2026-11-25: individual donors replaced by labels (Employee A/B, Fundraiser C); amounts are real. Decided 25/11 as options A/A/A (D24).]

Hi Nel,

From the 24 Nov review we agreed: giving history shows direct gifts and soft credits separately with a total for each; an amount field lets only part of a gift be soft credited (e.g. GBP 10 of GBP 20); Benevity matched gifts sit on the company Account with the employee soft credited. We also decided that if a soft credit names a Contact or Account that is not in CRM yet, it is created automatically.

Three points are still open. Below each question are real CRM records to walk through.

---------------------------------------------------------------------
WHAT CRM HOLDS TODAY (live figures, 25 Nov 2026)
---------------------------------------------------------------------
- 5,194 Donations carry a soft credit: 5,049 to a Contact (Soft Credit Contact), 145 to an Account (Soft Credit Account).
- No Donation has both, so today it is strictly ONE soft credit per gift.
- 244 Contacts and 17 Accounts receive soft credits.
- No soft credit has an amount: each one implicitly means "the whole gift".
- Lifetime Giving today counts DIRECT gifts only (the payer). Soft credits are not counted anywhere yet.

---------------------------------------------------------------------
QUESTION 1. Can one gift be soft credited to more than one person or organisation?
---------------------------------------------------------------------
Options
- A. One per gift: keep Soft Credit Contact / Soft Credit Account, add a Soft Credit Amount field. About 3.5 days.
- B. Several per gift: a list of soft-credit lines under each Donation (who + amount per line). About 7 days.

Example 1a - how a multi-funder payment is handled today (works with A)
  MicroLoan Foundation USA sent internal transfers on 28/07/2025. Instead of one Donation with several credits, each funder got its own Donation:
    Internal Transfer 28/07/2025   GBP 13,473.36   soft credit: Funder 1 (a family foundation)
    Internal Transfer 28/07/2025   GBP  5,613.90   soft credit: WIL of Greater Philadelphia
    Internal Transfer 28/07/2025   GBP  1,497.04   soft credit: Funder 1 (a family foundation)
  Splitting the payment into one Donation per credited party keeps option A workable for this case.

Example 1b - where option B would be needed
  A Benevity payment of GBP 100 = GBP 50 from employee X + GBP 50 company match, and the company is also to be credited separately from another party (e.g. a fundraiser). Under A you would split the GBP 100 into two Donations; under B you keep one Donation with two credit lines.
  Question for you: how often does one payment need to credit two DIFFERENT people/organisations? If it is rare, A plus splitting is enough.

Note on joint donors: couples are stored as one Contact (e.g. "<first name> and <first name> <surname>", 39 gifts, GBP 3,296.57 soft credited), so they do not need two credits.

---------------------------------------------------------------------
QUESTION 2. What do we do with the 5,194 existing soft credits?
---------------------------------------------------------------------
Options
- A. Backfill Soft Credit Amount = 100% of the gift (quick, keeps today's meaning).
- B. Leave blank and review by hand (slow: 5,194 records).

Example 2a - Employee A / Visa Europe (backfill at 100% looks right)
  Visa Europe (Account) has 10 gifts soft credited to Employee A, total GBP 16,486.79, e.g.
    29/08/2026  GBP   553.69
    30/12/2025  GBP 1,200.00
    21/12/2018  GBP 4,000.00
  If each is Visa matching Employee A's own giving, 100% is correct.
  If some Visa payments also matched OTHER employees, 100% over-credits Employee A: e.g. if the GBP 4,000 covered Employee A GBP 2,000 + others GBP 2,000, only GBP 2,000 should be theirs. Do you know?

Example 2b - Employee B / Deutsche Bank (backfill at 100% looks right)
  Deutsche Bank (currently a Contact, moving to Accounts under A5) pays GBP 17.14 a month, all 67 gifts soft credited to Employee B, total GBP 1,347.11. One payer, one beneficiary: 100% is right.

Example 2c - Fundraiser C (fundraiser; 100% right, but see Q3)
  127 gifts from 84 different supporters soft credited to Fundraiser C, total GBP 4,022 (e.g. one supporter GBP 1,020; many GBP 10-20 gifts). Each gift was made "for" their fundraising, so 100% is right.

---------------------------------------------------------------------
QUESTION 3. Do soft credits count in Lifetime Giving?
---------------------------------------------------------------------
Options
- A. No: Lifetime Giving stays direct gifts only; a separate Soft Credit Total sits next to it.
- B. Yes: Lifetime Giving = direct + soft credits.

Worked figures (live CRM values)

  Contact / Account          Direct (Lifetime today)   Soft credits received   Option A shows          Option B shows
  Employee A               GBP 15,470.75             GBP 16,486.79 (10)      15,470.75 + 16,486.79   GBP 31,957.54
  Visa Europe (Account)      GBP 22,281.06             GBP  1,140.66 (19)      22,281.06 + 1,140.66    GBP 23,421.72
  Employee B                GBP  2,207.63             GBP  1,347.11 (67)      2,207.63 + 1,347.11     GBP  3,554.74
  Deutsche Bank (Contact)    GBP  4,594.25             -                       4,594.25                GBP  4,594.25
  Fundraiser C            GBP    300.00             GBP  4,022.00 (127)     300.00 + 4,022.00       GBP  4,322.00
  Simmons and Simmons (Acc.) GBP  8,112.20             GBP  2,622.00 (69)      8,112.20 + 2,622.00     GBP 10,734.20

What changes with option B
- Double counting in any total: Visa's GBP 16,486.79 to Employee A is counted on Visa (payer) AND on Employee A. Adding up Lifetime Giving across donors (reports, Campaigns segments like "Lifetime > GBP 10k") over-states income.
  Example: Employee A moves from GBP 15k to GBP 32k and could enter a major-donor segment on gifts Visa paid.
- Self-credit: Fundraiser C gave GBP 50 themself and that same gift is also soft credited to them. Under B they are counted twice for it (GBP 50 direct + GBP 50 soft). These cases would need cleaning first.
- Segments that should reach the person who raised the money (e.g. fundraisers like Fundraiser C: GBP 300 direct, GBP 4,022 raised) are easier under B, but option A gives the same result by filtering on Soft Credit Total.

---------------------------------------------------------------------
HOW EACH CHOICE AFFECTS FUTURE SOFT CREDIT UPDATES
---------------------------------------------------------------------
- New Benevity / matched gift (one per gift, A): import the Donation on the company Account; Soft Credit Contact = employee; Soft Credit Amount = matched part (e.g. GBP 50 of GBP 100). The nightly refresh adds GBP 50 to the employee's Soft Credit Total; the company's Lifetime Giving gets the full GBP 100.
- Missing employee/company: created automatically (decided), matched first by email (Contacts) or exact name (Accounts) to avoid duplicates.
- Editing a soft credit (changing person or amount): the next nightly run recalculates both the old and the new person.
- Under Q1 option B, the import template needs extra rows per Donation (one per credit line), which is harder for peers to fill in.

---------------------------------------------------------------------
RECOMMENDATION
---------------------------------------------------------------------
1. One soft credit per gift (A); split a payment into separate Donations when it must credit two parties.
2. Backfill the existing 5,194 at 100%; review only the Visa Europe gifts to Employee A if some payments covered other staff.
3. Keep Lifetime Giving to direct gifts; add a separate Soft Credit Total (and show both on the widget).

Why now: the import templates need the field names early. With these answers the only new field is Soft Credit Amount on Donations (Soft Credit Contact and Soft Credit Account already exist), plus Soft Credit Total on Contacts and Accounts for the giving history.

Thanks,
Vijay
