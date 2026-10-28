# Óptica Global · Full test plan for a live ecommerce

**Role:** freelance QA, working alone with the dev team · **When:** 2025 to present · **Product:** [Óptica Global](https://opticaglobal.store/ar), a B2B wholesale eyewear store built on Medusa JS, live in production

This is the real plan I wrote and ran, exported from the spreadsheet I work in. The cases and bug reports are in Spanish because that's the language of the client and the team. Customer emails, order IDs and links to private evidence were replaced with generic values; everything else is as I wrote it.

| File | What's in it |
|---|---|
| [`test-plan.md`](test-plan.md) | Scope, what's out, and the business rules of this store that changed the plan |
| [`test-cases.md`](test-cases.md) | All 111 cases by area, with steps, expected result, priority, type, status and linked bug |
| [`test-cases.csv`](test-cases.csv) | The same table, to open in a spreadsheet |
| [`bug-reports.md`](bug-reports.md) | All 23 bug reports with environment, steps, actual vs. expected, evidence and dated retests |
| [`api-testing.md`](api-testing.md) | How I used the real API to check what the screen doesn't show: business rules, concurrency, backend confirmation, Admin guardrails |
| [`evidence/`](evidence/) | A cleaned sample of the real evidence: a screenshot and API responses |
| [`improvements.md`](improvements.md) | 3 product suggestions, kept apart from the bugs |

## How the plan was built

I started from a generic Medusa JS ecommerce plan of 56 cases, then **explored the real store before running anything** and rewrote it around how this business actually works:

- There's no guest checkout: customers identify with an email code, and the account is created by the first order.
- No coupons. Discounts are automatic, by volume.
- Payment is a manual bank transfer confirmed by WhatsApp, and shipping is coordinated with one carrier.

That turned generic cases ("apply a coupon," "pay with a card") into cases that could actually fail here, and the plan grew to **111 cases across 12 areas**.

## Coverage and results

| Area | Cases | Passed | Failed | Blocked | Pending |
|---|---|---|---|---|---|
| Checkout | 15 | 11 | 3 | 1 | |
| Payments | 10 | 10 | | | |
| User accounts | 10 | 6 | 3 | 1 | |
| Emails and notifications | 12 | 10 | | 2 | |
| Store API (Medusa) | 9 | 8 | | | 1 |
| Admin panel | 8 | 4 | 1 | 3 | |
| Shipping | 4 | 4 | | | |
| Catalog | 6 | 4 | | 2 | |
| Returns and refunds | 9 | 4 | | 2 | 3 |
| Volume discounts | 10 | 3 | | 7 | |
| SEO | 10 | 1 | 9 | | |
| Referrals | 8 | 1 | 1 | | 6 |
| **Total** | **111** | **66** | **17** | **18** | **10** |

**How they were run:** 47 by hand in the browser, 41 against the real API ([how and why](api-testing.md)), 13 automated with Playwright. The automated ones are in [optica-global-playwright-tests](https://github.com/micaelajara/optica-global-playwright-tests).

**Blocked doesn't mean skipped.** Every blocked case says why: a scenario that doesn't apply to a wholesale business, a catalog too small to have a second page, an order already delivered that can't be cancelled. Seven of them are volume discount cases whose premise turned out to be wrong (more on that below).

## Bugs

**23 reported, 21 confirmed by the dev team, 2 closed as expected behavior.** By severity, the confirmed ones are 1 critical, 5 high, 11 medium and 4 low.

### The critical one: a discount nobody could use

Customers referred by an optical lab get a discount on their first purchase. The referral code could only be entered in "My profile." The profile only exists once you have an account. The account is only created by completing a first purchase. So by the time a customer could enter the code, the only purchase that qualified for it was already gone.

Each screen worked fine on its own; the bug was in the chain. After a partial fix added a code field to checkout, I retested with a real order and checked the backend: the discount was still not being applied, because the link between customer and lab wasn't being saved. It stayed open, with the evidence. ([BUG-15](bug-reports.md#bug-15))

### Others worth reading

- **Race condition on stock** (CO-06, passed): I simulated two customers competing for the last 5 units through the API. The second checkout was rejected with "Not enough stock," so the store doesn't oversell.
- **No email when a refund is processed** ([BUG-21](bug-reports.md#bug-21), fixed): customers were told their order was cancelled but never that their money was returned.
- **A cancelled order still showed as successful** to the customer, even after reloading ([BUG-20](bug-reports.md#bug-20), fixed).
- **The profile always said "Email updated successfully"**, no matter which field you edited ([BUG-13](bug-reports.md#bug-13)).
- **The quantity selector let you add 21+ units with no stock check** ([BUG-18](bug-reports.md#bug-18), fixed).

### The two that were rejected

I reported that the volume discount promotions had no minimum quantity rule ([BUG-11](bug-reports.md#bug-11)) and that a discount applied in the cart was lost when the order was placed ([BUG-22](bug-reports.md#bug-22)). The developer showed me that the quantity rule lives in custom code, not in Medusa's promotion settings I was reading, and that my test cart had 11 units against a minimum of 50: the system removed the discount on purpose.

I kept both reports here, marked as rejected, and updated the seven cases built on that wrong premise. Getting the business rule wrong was my mistake; documenting it and fixing the plan is part of the job.

## Improvements suggested

A product search, a clear "create account" option outside checkout, and a "back to the store" button after placing an order. Details in [`improvements.md`](improvements.md).
