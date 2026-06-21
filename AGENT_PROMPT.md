# Daily Finance Internship Search

Run this workflow once, start to finish, and then stop.

## 1. Load inputs

- Read `firms.json` for the target firm list (banks, asset managers, hedge funds/prop trading, middle market private equity).

## 2. Search for listings

Target recruiting cycle: **Summer 2027** internships.

Target roles: any finance-related summer analyst / internship role, including but not limited to investment banking, wealth management, asset management, sales & trading, equity/credit research, private equity, and other summer analyst programs. Exclude roles that are clearly non-finance (e.g. pure software engineering, HR, marketing) unless the listing itself is a finance-track program.

Location: **US-based roles only.** Every search must specify a US location, and any extracted listing whose location is outside the United States must be discarded.

For every firm in `firms.json`:
- Use WebSearch (and WebFetch on promising result pages) to find current Summer 2027 internship postings on that firm's own careers site, e.g. `site:<firm-careers-domain> summer 2027 internship United States`, `site:<firm-careers-domain> summer analyst 2027 United States`.
- If a firm's career site search returns nothing or a fetch fails, skip that firm for this run and note it in your final run summary (do not fail the whole run).

Additionally, run a small set of aggregator searches covering all firms at once:
- `site:linkedin.com/jobs finance summer analyst 2027 United States`
- `site:indeed.com finance internship summer 2027 United States`
- general web search for "finance summer analyst internship 2027 United States" plus each firm name if the firm-specific search came up empty.

For each posting found, extract:
- `firm`
- `title`
- `location` (must be a US city/state)
- `link` (the posting URL)
- `description` — 1-2 sentences covering role focus (e.g. IB, wealth management, asset management, sales & trading, private equity) and any stated eligibility/class-year requirement

Only keep postings that are clearly tied to one of the firms in `firms.json`, are finance-related (per the role scope above), are US-based, and are for the Summer 2027 cycle (dedupe firm name spelling/casing as needed).

Include every matching listing found this run — do not filter out listings just because they were also found in a previous day's run.

## 3. Create the summary draft

Compose an email body grouped by firm, each entry showing title, location, link, and description. If no listings were found this run, create a draft saying so rather than skipping it.

Call the Gmail MCP `create_draft` tool, addressed to `ccmaxgu@gmail.com`, subject `Finance Internship Postings - <today's date, e.g. 2026-06-21>`.

## 4. Final summary

Output a short summary of the run: how many firms were searched, how many were skipped (and why), how many listings were found, and confirmation the draft was created.
