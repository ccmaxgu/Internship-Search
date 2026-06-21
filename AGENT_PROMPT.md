# Daily Finance Internship Search

Run this workflow once, start to finish, and then stop.

## 1. Load inputs

- Read `firms.json` for the target firm list (banks, asset managers, hedge funds/prop trading).

## 2. Search for listings

Target recruiting cycle: **Summer 2028** internships.

Target roles: any finance-related summer analyst / internship role, including but not limited to investment banking, wealth management, asset management, sales & trading, equity/credit research, and other summer analyst programs. Exclude roles that are clearly non-finance (e.g. pure software engineering, HR, marketing) unless the listing itself is a finance-track program.

For every firm in `firms.json`:
- Use WebSearch (and WebFetch on promising result pages) to find current Summer 2028 internship postings on that firm's own careers site, e.g. `site:<firm-careers-domain> summer 2028 internship`, `site:<firm-careers-domain> summer analyst 2028`.
- If a firm's career site search returns nothing or a fetch fails, skip that firm for this run and note it in your final run summary (do not fail the whole run).

Additionally, run a small set of aggregator searches covering all firms at once:
- `site:linkedin.com/jobs finance summer analyst 2028`
- `site:indeed.com finance internship summer 2028`
- general web search for "finance summer analyst internship 2028" plus each firm name if the firm-specific search came up empty.

For each posting found, extract:
- `firm`
- `title`
- `location`
- `link` (the posting URL)
- `description` — 1-2 sentences covering role focus (e.g. IB, wealth management, asset management, sales & trading) and any stated eligibility/class-year requirement

Only keep postings that are clearly tied to one of the firms in `firms.json` and are finance-related (per the role scope above) for the Summer 2028 cycle (dedupe firm name spelling/casing as needed).

Include every matching listing found this run — do not filter out listings just because they were also found in a previous day's run.

## 3. Send the summary email

Compose an email body grouped by firm, each entry showing title, location, link, and description. If no listings were found this run, send a short email saying so rather than skipping it.

Send the email via SMTP using Python's `smtplib` (invoke with the Bash tool), not the Gmail MCP draft tool:

- SMTP server: `smtp.gmail.com`, port `587`, STARTTLS
- From / login account: `ccmaxgu@gmail.com`
- App password: provided in the run instructions for this task (do not hardcode it in any repo file)
- To: `ccmaxgu@gmail.com`
- Subject: `Finance Internship Postings - <today's date, e.g. 2026-06-21>`

Write a short inline Python script, run it with `python3 -c "..."` or a temp script file, and confirm the SMTP call succeeded (no exception) before reporting success.

## 4. Final summary

Output a short summary of the run: how many firms were searched, how many were skipped (and why), how many listings were found, and confirmation the email was sent.
