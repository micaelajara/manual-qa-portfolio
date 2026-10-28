# Manual QA portfolio

Real manual and functional testing work: test plans, test cases, bug reports and the reasoning behind them. My automation repos show I can write code; this one shows how I decide what to test, how I run it, and how I report what I find.

**Micaela Jara** · QA Engineer · Buenos Aires · [Portfolio](https://micaelajara.github.io) · [LinkedIn](https://www.linkedin.com/in/micaela-jara/)

## What's inside

| Folder | What it is | Highlights |
|---|---|---|
| [**optica-global/**](optica-global/) | The full test plan I wrote and ran for a live B2B ecommerce (Medusa JS), as a freelance QA | 111 cases in 12 areas, 23 bug reports with evidence and retests, [API checks](optica-global/api-testing.md) on what the screen doesn't show, 3 product suggestions |
| [**credentialtracker/**](credentialtracker/) | A QA challenge on a healthcare credentialing app, tested across the UI, the API and the database | 8 findings ranked by real-world impact, including exposed medical PII and dashboard metrics that hid expired licenses |
| [**rebelmouse/**](rebelmouse/) | 4 years as the QA for the enterprise accounts of a SaaS CMS, written up without client material | Regression and smoke routines, a critical booking bug for the biggest client, SEO validation of entire sites |
| [**cross-device/**](cross-device/) | Practice: cross-device test design for a news site | A tiered device matrix, the breakpoints where layouts break, and 31 cases on layout, touch, Safari iOS and slow networks |
| [**templates/**](templates/) | The bug report and test case formats I use on every project | Why "Environment" and "Ruled out" are mandatory fields |

## Where to start

- **5 minutes:** the [Óptica Global overview](optica-global/README.md), then one bug report, for example [BUG-15](optica-global/bug-reports.md#bug-15), the critical one.
- **15 minutes:** the [CredentialTracker report](credentialtracker/README.md), from the application overview to the approach section at the end. It's the clearest example of how I go from understanding a domain to deciding severity.
- **API testing:** [how I used the real API](optica-global/api-testing.md) to confirm in the backend what the screen claims, including the check that kept BUG-15 open.
- **Just the format:** the [bug report template](templates/bug-report.md).

## How I work

1. **Understand** the feature and the business rule before writing a case. When I get a rule wrong, I say so and fix the plan (there's a real example in Óptica Global).
2. **Explore** the product before scripting anything.
3. **Plan** by business impact: scope, what's out, what goes first.
4. **Design** positive, negative and edge cases.
5. **Execute** and keep evidence: screenshots, videos, API responses.
6. **Report** bugs a developer can reproduce without asking me anything.
7. **Retest** every fix, and write down the result even when it's "still broken."

## A note on AI

I use Claude Code as a working tool: to help draft cases and reports, and to turn spreadsheets and notes into documents like these. I decide what to test, I run the tests, and I confirm every bug myself before it goes into a report.
