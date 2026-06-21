# Daily Finance Internship Search

Run this workflow once, start to finish, and then stop. Working directory: `/Users/ccmaxgu/Intern Search`.

## 1. Load state

- Read `firms.json` for the target firm list (banks, asset managers, hedge funds/prop trading).
- Read `seen_listings.json` for previously-seen listings (keys are listing URLs, or a `firm|title|location` string if no stable URL exists; values are the date first seen).

## 2. Search for listings

For every firm in `firms.json`:
- Use WebSearch (and WebFetch on promising result pages) to find current internship postings on that firm's own careers site, e.g. `site:<firm-careers-domain> internship 2027`.
- If a firm's career site search returns nothing or a fetch fails, skip that firm for this run and note it in your final run summary (do not fail the whole run).

Additionally, run a small set of aggregator searches covering all firms at once:
- `site:linkedin.com/jobs finance internship 2027`
- `site:indeed.com finance internship 2027`
- general web search for "finance internship 2027" plus each firm name if the firm-specific search came up empty.

For each posting found, extract:
- `firm`
- `title`
- `location`
- `link` (the posting URL)
- `description` — 1-2 sentences covering role focus and any stated eligibility/class-year requirement

Only keep postings that are clearly tied to one of the firms in `firms.json` (dedupe firm name spelling/casing as needed).

## 3. Diff against seen listings

- Build a key for each extracted listing: prefer the posting URL; if a firm reposts the same role under a tracking-parameter-mutated URL, normalize by stripping query strings before keying.
- Compare against `seen_listings.json`. Keep only listings whose key is NOT already present.

## 4. Send the summary (only if there are new listings)

If there is at least one new listing:
- Compose an email body grouped by firm, each entry showing title, location, link, and description.
- Call the Gmail MCP `create_draft` tool, addressed to `ccmaxgu@gmail.com`, subject `Finance Internship Postings - <today's date, e.g. 2026-06-21>`.
- Only after the draft is created successfully, write the new listings into `seen_listings.json` (merge with existing entries, don't drop old ones), each with today's date as the first-seen value.

If there are no new listings:
- Do not create a draft.
- Do not modify `seen_listings.json`.

## 5. Final summary

Output a short summary of the run: how many firms were searched, how many were skipped (and why), how many new listings were found, and whether a draft was created.
