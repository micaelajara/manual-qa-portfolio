# Evidence

A sample of the real evidence behind the bug reports and cases. The full set (videos, every API dump) stays private because it contains customer and store data. What's here was cleaned: IDs, emails, phone numbers and tax IDs are replaced or blurred, everything else is as captured.

| File | Backs up | What it shows |
|---|---|---|
| [`BUG-01-checkout-overflow.png`](BUG-01-checkout-overflow.png) | [BUG-01](../bug-reports.md#bug-01) | Retest on a real confirmed order (2026-07-06): a long address runs over the Contact column. Contact data blurred |
| [`BUG-15-lab-discount-not-applied.json`](BUG-15-lab-discount-not-applied.json) | [BUG-15](../bug-reports.md#bug-15) | The order with no discount, the customer with no lab linked, and the labs with 0 referred customers, after the partial fix |
| [`AD-04-AD-06-admin-guardrails.json`](AD-04-AD-06-admin-guardrails.json) | AD-04, AD-06 | Three Admin actions correctly blocked, with the exact error bodies |
| [`BUG-11-promotions-config.json`](BUG-11-promotions-config.json) | [BUG-11](../bug-reports.md#bug-11) (rejected) | The promotion config I misread. Kept because it explains the mistake |

How each one was used is in [api-testing.md](../api-testing.md).
