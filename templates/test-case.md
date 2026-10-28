# Test case template

## From a single ticket

| ID | Description | Steps | Expected result | Type |
|---|---|---|---|---|
| CO-03 | Try to reach checkout with an empty cart | 1. Empty the cart<br>2. Go to /checkout directly by URL | The system redirects to the cart or shows an empty cart message, and doesn't let the user continue | Negative |

**Type** is Positive, Negative or Edge. I aim for a mix: a plan with only happy paths isn't testing anything that's likely to break.

## For a multi-module plan

When there's no single ticket and the job is "test the whole store," I add three columns:

| Column | Why |
|---|---|
| **Area** | To group cases by module and see coverage per area at a glance |
| **Priority** (High, Medium, Low) | To decide execution order when there isn't time for everything, which is always |
| **Status** (Pending, Passed, Failed, Blocked) + **Executed by** + **Notes / Bug** | To turn the plan into an execution tracker and link each failure to its bug report |

Before writing a plan like this I ask two questions: what's in scope (and what was already tested), and what matters most to the business. Without that the plan grows too big to run. A real example is in [`../optica-global/`](../optica-global/).
