# RebelMouse · Building QA from scratch

**Role:** QA Engineer and Technical Support · **When:** March 2022 to May 2026 · **Product:** SaaS B2B CMS for enterprise publishers and brands

No test cases or bug reports from this job are published here: they belong to the company and its clients. This is a write-up of how I worked and what changed because of it.

## Context

When I joined there was no QA process, so I built it. I worked with three QA colleagues in Russia, with the clients and most of the company in the US. Everything (tickets, meetings, documentation, client calls) happened in English.

## What I built

- **Test cases** written from the tickets.
- A **regression pass before every release**.
- A **smoke test after every deploy** to production.
- A **bug flow with development**: how bugs get reported, prioritized and retested.
- **Proactive testing** when there were no tickets: exploring the product without a script, looking for problems nobody had asked about.

Releases ran through TeamCity, which was my first hands-on contact with CI.

## Penske Truck Rental

The company's biggest client: 5 domains with custom code integrated into the CMS. I covered new features, full redesigns and pre-launch validation on every development cycle, and improved the way test cases were presented to the client so they could see what had been covered before a launch.

**The bug I remember most:** a critical failure in the **truck booking flow**, the feature with the highest business impact for the client. Instead of reporting "booking doesn't work," I opened the network tab, followed the requests until I found the endpoint that was failing, and escalated it with that evidence and the business impact spelled out. It was fixed within hours. For Penske, an hour of broken booking was an hour of real operational impact, and that's how I learned to prioritize: by what it costs the client, not by how interesting the bug is.

## SEO validation across entire sites

When there were no tickets, one of the things I went looking for was SEO problems. I validated whole sites with a Python script and Screaming Frog, a crawler that walks every page of a site. It's the same kind of check I later turned into Playwright tests for Óptica Global.

## Technical support for J.J. Keller

The other half of the role. J.J. Keller, a compliance company, was an enterprise account I supported directly by chat, email and Zoom. The core skill was diagnosis, which is the same skill as testing:

- **Platform bug, client configuration, or a failing integration?** Answering that question correctly decides who has to act, and getting it wrong wastes everyone's day.
- Troubleshooting third-party integrations, Okta among them.
- Onboarding and training non-technical users, which taught me to explain a problem in the client's words and not in the system's.

## What I took from it

- A process is only useful if the team actually uses it, so I built it around what developers and clients already did, not around a template.
- Severity comes from business impact. The same bug is critical on a booking flow and minor on a settings page.
- Support and QA are the same job from two sides: both start with "what exactly is happening, and how do I prove it."
