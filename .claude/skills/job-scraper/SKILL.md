---
name: job-scraper
description: >
  Scrapes configured job sites for new positions matching your profile. Deduplicates across runs.
  Triggers on: job scrape, find jobs, search jobs, new jobs, job search, scrape jobs, /scrape
allowed-tools: Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, Agent, AskUserQuestion
---

# Job Scraper

---

## How It Works

This skill searches the job sites and pages configured in `search-queries.md` using targeted queries based on your profile, deduplicates against previously seen jobs and the application tracker, and presents new matches with a quick fit assessment.

## Invocation

The user triggers this skill by saying things like:
- "Find new jobs"
- "Scrape for jobs"
- "Any new positions?"
- "/scrape"

Optional arguments:
- A focus area, e.g. "/scrape data science" or "/scrape geophysics"
- "broad" to run all search categories, e.g. "/scrape broad"

---

## Execution Steps

### Step 0: Load State

1. Read `job_scraper/seen_jobs.json` (create if missing - start with `{"seen": {}}`)
2. Read `job_search_tracker.csv` to extract already-applied companies+roles
3. Read `search-queries.md` (this directory) for the search strategy

### Step 1: Search

Run the sources from `search-queries.md` for the top 3 priority categories by default. If the user said "broad", run all categories. If the user specified a focus area (e.g. "data science"), prioritize queries/pages from that category.

- **LinkedIn (primary):** `WebFetch` the role+city list pages listed in `search-queries.md` directly - do NOT `site:` search LinkedIn via WebSearch, it mostly returns expired listings or generic category pages rather than live postings. Ask WebFetch to list every posting shown (title, company, location, date, URL) from each list page.
- **eFinancialCareers / infojobs.net (secondary):** Use `WebSearch` with the `site:` queries from `search-queries.md`. These sites block WebFetch (HTTP 405) - treat every hit as unverified.
- **Known-employer career pages (tertiary):** Occasionally `WebFetch` the companies listed in `search-queries.md` directly.

Target your configured geographic area. Look for postings from the last 14 days.

### Step 2: Fetch & Parse

For each promising result from Step 1:
- Use `WebFetch` on the individual job URL to verify it and extract: **job title**, **company**, **location**, **posting date** (or "recent"), **URL**, **key requirements** (brief), **application deadline** (if listed)
- Skip if the URL or company+title combo already exists in `seen_jobs.json`
- Skip if the company+role already appears in `job_search_tracker.csv`
- **If the fetch redirects to an expired/generic page** (e.g. LinkedIn's `expired_jd_redirect`) or **returns HTTP 405/blocked** (eFinancialCareers, infojobs.net) or **returns no usable content** (JS-rendered career pages like Workday): do not include the job in the verified results table. Instead:
  - Expired/dead listings → record in `seen_jobs.json` as `status: "skipped-expired"` and drop entirely, don't mention in the digest
  - Blocked/unverifiable but the listing looks like a real, relevant, open posting (from the WebSearch snippet or list-page text) → keep it, but only in a separate "found but could not verify automatically - check manually" section of the digest with its URL and the reason, never in the main matches table
  - Do not retry a source that just failed with 405/blocked within the same run - it's a consistent block, not a transient error

### Step 3: Quick Fit Assessment

For each new job, do a rapid fit check (NOT the full evaluation from `04-job-evaluation.md` - just a quick signal):

- **High match**: Role directly involves your core skills
- **Medium match**: Role is adjacent to your experience
- **Low match**: Role requires significant skills you lack

### Step 4: Deduplicate & Store

1. Add ALL fetched jobs (new and skipped) to `seen_jobs.json` with structure:
```json
{
  "seen": {
    "<url_or_company_title_key>": {
      "title": "...",
      "company": "...",
      "url": "...",
      "first_seen": "YYYY-MM-DD",
      "fit": "high/medium/low",
      "status": "new/skipped/evaluated"
    }
  }
}
```
2. Only present jobs NOT already in the seen list or tracker.

### Step 5: Present Results

Present new jobs in a table sorted by fit (high first):

```
## New Job Matches - YYYY-MM-DD

Found X new positions (Y high, Z medium, W low match).

| # | Fit | Title | Company | Location | Deadline | URL |
|---|-----|-------|---------|----------|----------|-----|
| 1 | High | ... | ... | ... | ... | [Link](...) |

### High-Match Highlights
For each high-match job, add 2-3 bullet points:
- Why it matches your profile
- Key requirements to check
- Any red flags

### Found but couldn't verify automatically
List any leads from blocked/unverifiable sources here (eFinancialCareers, infojobs.net, JS-rendered career pages), with title, company, location, URL, and the reason verification failed. These are not in the table above and should not be treated as confirmed-open.
```

After presenting, ask:
> "Want me to evaluate any of these in detail? Just give me the number(s)."

If the user picks a number, invoke the **job-application-assistant** skill workflow (fit evaluation first, then CV + cover letter if approved).

### Step 6: Update Tracker (Optional)

If the user decides to apply to any job, add a row to `job_search_tracker.csv`.

---

## Important Rules

1. **Never fabricate job postings.** Only present jobs found via actual WebSearch/WebFetch results.
2. **Respect deduplication.** Always check seen_jobs.json AND job_search_tracker.csv before presenting.
3. **Focus on configured geographic area.** Skip jobs that require relocation or are clearly outside commute range.
4. **Only open positions.** Skip postings with expired deadlines or those marked as closed.
5. **Be efficient with WebFetch.** Don't fetch every search result - use titles and snippets to pre-filter before fetching.
6. **Parallel searches.** Use the Agent tool or parallel WebSearch/WebFetch calls to speed up the search phase.
7. **Never include an unverified posting in the main matches table.** If WebFetch can't confirm a listing is real and open (blocked, expired, no content), it goes in "Found but couldn't verify automatically" instead, or is dropped if clearly dead.
