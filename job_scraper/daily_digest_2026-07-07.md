# Daily Job Digest - 2026-07-07

## Summary

Ran the top 3 priority search categories (Tech M&A Analyst/Associate, Investment Banking Analyst, Corporate Development/Strategy) restricted to Madrid and London, per `.claude/skills/job-scraper/search-queries.md`.

**Result: 0 new verified, open job matches today.**

13 candidate postings were surfaced via WebSearch and individually checked via WebFetch. All were excluded before reaching the digest:

| Title | Company | Location | Reason excluded |
|---|---|---|---|
| M&A Associate - Generalist | Pearse Partners | Madrid | Expired (LinkedIn redirect) |
| Corporate Development Associate - AVP | Barclays | London | Expired (LinkedIn redirect) |
| AVP Corporate Development Associate | Lockton | London | Expired (LinkedIn redirect) |
| M&A Intern-to-Analyst / Analyst 1/2 - Mid-market TMT Advisory | Unnamed (via eFinancialCareers) | London | Expired (LinkedIn redirect) |
| Technology M&A Senior Associate and VP - Bulge Bracket IB | Unnamed (via eFinancialCareers) | London | Expired (404) |
| Energy & Infrastructure M&A Analyst/Associate | Unnamed mid-market M&A advisor | London | Expired (LinkedIn redirect) |
| M&A Strategy Manager | Accenture (via eFinancialCareers) | Madrid | Could not verify - eFinancialCareers blocked automated fetch (HTTP 405) |
| Investment Banking Analyst, Consumer M&A, European Investment Bank | Unnamed (via eFinancialCareers) | London | Could not verify - fetch blocked (HTTP 405) |
| Investment Banking - EMEA M&A - Associate | Unnamed (via eFinancialCareers) | London | Could not verify - fetch blocked (HTTP 405) |
| Finance Analyst (M&A) - Financial Services | Unnamed (via eFinancialCareers) | London | Could not verify - fetch blocked (HTTP 405) |
| Insurance M&A Analyst | Unnamed (via eFinancialCareers) | London | Could not verify - fetch blocked (HTTP 405); also insurance-focused, not tech |
| Investment Banking Associate, Technology M&A | Citi | London | Could not verify - fetch returned unrelated search page |
| Investment Banking Analyst - Technology | Alantra | Madrid | Could not verify - Workday careers page is JS-rendered, fetch returned no content. Strong profile fit (Madrid-based tech IB boutique) - **worth checking manually** at https://alantra.wd3.myworkdayjobs.com/en-US/Alantra/job/Investment-Banking-Analyst---Technology_JR243 |

## Notes

- Many LinkedIn search results this run resolved to expired listings (LinkedIn's `expired_jd_redirect`) or generic category-list pages rather than individual postings - a known limitation of `site:` search against LinkedIn.
- eFinancialCareers blocked all automated fetches today (HTTP 405 on every URL tried), so postings sourced from it could not be independently verified and were excluded per the verification rule in the job-scraper skill.
- Other candidates surfaced in search snippets but not fetched (filtered out before spending a fetch): graduate/summer analyst programmes (doesn't match Juan's current experience level), Director-level corporate development roles (too senior), and general business-development/sales roles (not a fit).
- All 13 checked postings have been recorded in `job_scraper/seen_jobs.json` so they won't be re-checked on the next run.
- The Alantra Madrid Tech IB Analyst posting is a strong profile match on paper (Madrid-based, tech-focused boutique) - flagging for manual review since automated verification wasn't possible.

No jobs met the bar for inclusion in today's results table.
