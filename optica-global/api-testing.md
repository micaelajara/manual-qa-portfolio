# API testing · Óptica Global

41 of the 111 cases were run against the real Medusa JS API instead of (or on top of) the browser. Not to automate them: to check things the screen can't show me. A confirmation page can say "order placed" while the backend saved something different, and a button can be hidden while the endpoint behind it is still open.

**Tools:** curl and the browser's network tab. Store API calls with the store's publishable key (the same one the storefront uses), Admin API calls with an admin token. Every request below was run against the live store; IDs and keys are replaced with placeholders.

```bash
# Store API: what a customer's browser can do
curl "$API/store/products" -H "x-publishable-api-key: $PK"

# Admin API: what the shop owner sees in the panel
curl "$API/admin/orders?fields=*promotions" -H "Authorization: Bearer $ADMIN_TOKEN"
```

## 1. Rules the UI doesn't enforce

The first question for any rule: if I skip the screen and call the endpoint directly, does it still hold?

| Case | What I sent | What came back | Verdict |
|---|---|---|---|
| API-03 | `POST /store/carts/{id}/line-items` with a variant that has 0 stock | `400 insufficient_inventory` | Pass. Stock is checked in the backend, not only by a disabled button |
| API-05 / API-09 | `POST /store/carts/{id}/complete` with a small cart and no payment data | `"El pedido mínimo es 150000. Te faltan 135500 para poder finalizar la compra."` | Pass, but this minimum order rule wasn't documented anywhere. I added it to the plan and flagged that the storefront should show it before checkout |
| API-06 | `GET /store/customers/me` with no customer token | `401 Unauthorized` | Pass |
| SH-04 | Disabled the only shipping method in Admin while a cart already had it selected, then completed the cart | `"No shipping method selected but the cart contains items that require shipping"` | Pass. The cart didn't keep a method that no longer exists |

## 2. Concurrency

Two things a single tester clicking through a browser can't reproduce.

| Case | What I did | Result |
|---|---|---|
| CO-06 | Two customers, two carts, competing for the last 5 units. Completed cart B, then tried to complete cart A | Cart A rejected with `"Not enough stock available for item..."`. The store doesn't oversell |
| API-07 | 5 parallel requests adding the same variant to the same cart | Merged into 1 line with quantity 5. No duplicated lines, no corrupted cart |

## 3. Confirming in the backend what the screen claims

| Case | The screen said | The API showed |
|---|---|---|
| EM-01 | "Order confirmed" | `metadata.confirmation_email_sent_at` set on the order, so the email was actually triggered |
| EM-03, SH-03 | Order shipped and delivered | The fulfillment with its tracking number, and `shipped_email_fulfillments` / `delivered_email_fulfillments` in the order metadata |
| AD-02, CB-03 | Stock set to 0 in Admin | The storefront showed "sin stock" about **1 minute later**, not instantly. Not a bug, but worth knowing: Next.js caches the page, so a customer can add an item that just ran out |
| [BUG-15](bug-reports.md#bug-15) | After a partial fix, checkout had a field for the lab referral code and the order went through | The order had `discount_total: 0` and `promotions: []`, the customer had no lab linked, and both labs still had `active_optica_count: 0`. The field was there, but nothing was being saved. **The bug stayed open because of this check.** [Evidence](evidence/BUG-15-lab-discount-not-applied.json) |

## 4. Guardrails in the Admin

Negative cases on actions that should be blocked. The error messages are the evidence. [All three responses](evidence/AD-04-AD-06-admin-guardrails.json)

| Case | Action | Response |
|---|---|---|
| AD-06 | Cancel an order that has an active fulfillment | `not_allowed: All fulfillments must be canceled before canceling an order` |
| AD-06 | Cancel a fulfillment that was already shipped | `not_allowed: The fulfillment has already been shipped. Shipped fulfillments cannot be canceled` |
| AD-04 | Add a manual discount (credit line) to an order | `invalid_data: Can only create credit lines if the order has a positive or negative pending difference`. Blocked, not failed: it needs an order edit first, which was out of scope |
| DEV-07 | Request a second return while one is still active | `already has an existing active order change` |

## 5. Where reading the API led me wrong

`GET /admin/promotions` showed the volume discounts with `rules: []` and `is_automatic: false`. I read that as "these discounts have no minimum quantity" and reported it ([BUG-11](bug-reports.md#bug-11)). The developer showed me the minimum lives in custom code that applies the promotion, not in Medusa's promotion settings. [The response I read](evidence/BUG-11-promotions-config.json)

What I took from it: the API shows what is configured, not always what the business does. When the configuration and the expected behavior disagree, I now ask where the rule is implemented before calling it a bug.
