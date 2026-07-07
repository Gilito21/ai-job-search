# Search Queries for Job Scraper

## Search Sites

Juan is based in Madrid, Spain and open to relocation to London only. The built-in job-portal CLI tools in this repo (`jobindex-search`, `jobbank-search`, `jobdanmark-search`, `jobnet-search`) are **Danish-market-specific** and not relevant here.

### Primary: LinkedIn Jobs list pages (fetch directly, don't `site:` search)

`site:linkedin.com/jobs "..."` Google searches mostly return **generic category pages or expired individual listings** (LinkedIn's `expired_jd_redirect`) rather than live postings - this was confirmed across multiple scrape runs. Instead, **WebFetch the role+city list page directly** - these reliably return 5-15 real, currently-open, individually-linked postings with dates:

```
https://es.linkedin.com/jobs/m&a-associate-empleos-madrid
https://es.linkedin.com/jobs/investment-banking-analyst-empleos-madrid
https://es.linkedin.com/jobs/strategy-analyst-empleos-madrid
https://uk.linkedin.com/jobs/m&a-analyst-jobs-london
https://uk.linkedin.com/jobs/investment-banking-analyst-jobs-london
https://uk.linkedin.com/jobs/corporate-development-associate-jobs
```

Prompt WebFetch to "list all current postings shown: title, company, location, posting date, and URL for each." Then pre-filter that list by title/company relevance before individually WebFetching the most promising ones to verify details (per Step 2 of `SKILL.md`).

The `linkedin-search` CLI (`.agents/skills/linkedin-search/`) is also available and not region-locked - use it as an alternative if list-page fetching becomes unreliable.

### Secondary: eFinancialCareers - WebSearch only, never auto-included

`efinancialcareers.com` / `efinancialcareers.co.uk` cover IB/M&A roles well, but **block WebFetch entirely (HTTP 405)** - confirmed on every URL tried across multiple runs, including plain listing pages. Since the skill's "never fabricate" rule requires WebFetch verification before a posting appears in the results table, **any eFinancialCareers hit found via WebSearch must be excluded from the verified matches table** and instead listed in the digest's "worth checking manually" section with its URL. Do not keep retrying WebFetch against efinancialcareers.com/.co.uk - it fails consistently, not intermittently.

```
site:efinancialcareers.com "M&A" "Analyst" Madrid
site:efinancialcareers.co.uk "M&A" "Analyst" London
site:efinancialcareers.com "Investment Banking Analyst" Madrid
site:efinancialcareers.co.uk "Investment Banking Analyst" London
```

### Tertiary: Known relevant employers - direct career-page check

These companies have repeatedly posted strong-fit tech-adjacent M&A/corp-dev roles in Madrid or London. Worth an occasional direct career-page WebFetch alongside the LinkedIn list pages:
- Santander Corporate & Investment Banking (SCIB) - multiple M&A verticals in Madrid (Tech, TMT, Energy, Infrastructure)
- Fever - Madrid-based experiences-tech scaleup, repeat M&A/strategy hiring
- Verisure - Madrid-area smart-security tech company
- Nebius, William Blair - London

Some career pages (e.g. Workday-hosted ones like Alantra's) are JS-rendered and return no content via WebFetch. Don't drop these silently - flag them in the digest as "found but could not verify automatically, check manually" with the direct URL, same treatment as eFinancialCareers hits.

### infojobs.net - Spain general board (secondary net)

```
site:infojobs.net "analista M&A" OR "analista financiero" Madrid
```
Same caveat as eFinancialCareers: verify with WebFetch before including in the results table; if WebFetch fails, flag for manual review instead of dropping.

If Juan wants dedicated CLI scraping for eFinancialCareers or InfoJobs (to work around the WebFetch block), use `/add-portal` to generate a portal search skill for either site.

## Query Categories

Categories below map to the sources above. Each query/URL should be scoped to Madrid or London - the only two acceptable locations.

### Priority 1: Tech M&A Analyst / Associate

These match Juan's strongest and current career direction. Fetch the Priority 1 LinkedIn list pages (M&A Associate Madrid, M&A Analyst London) plus:
```
site:efinancialcareers.com "M&A" "Analyst" Madrid
site:efinancialcareers.co.uk "M&A" "Analyst" London
```

### Priority 2: Investment Banking Analyst (broader IB)

These match Juan's broader IB experience (Citi) and domain expertise in valuation/financial modelling. Fetch the Investment Banking Analyst list pages (Madrid + London) plus:
```
site:efinancialcareers.com "Investment Banking Analyst" Madrid
site:efinancialcareers.co.uk "Investment Banking Analyst" London
```

### Priority 3: Corporate Development / Strategy (adjacent pivot)

Adjacent roles at tech companies that Juan could pivot into. Fetch the Corporate Development Associate (UK) and Strategy Analyst (Madrid) list pages.

### Priority 4: Broader Finance / Consulting

Wider net for general finance and consulting roles that use similar skills.

```
site:linkedin.com/jobs "Financial Analyst" technology Madrid OR London
site:linkedin.com/jobs "Technical Consultant" finance Madrid OR London
site:infojobs.net "analista M&A" OR "analista financiero" Madrid
```

Note: the `site:linkedin.com/jobs` queries in Priority 4 inherit the same expired/generic-page problem noted above - treat any LinkedIn hit here as a lead to re-fetch via its role+city list page (Primary source), not as a direct WebFetch target.

## Location Filter

Juan is only open to Madrid and London - no other cities or countries.
- **Madrid, Spain** (ideal - current base)
- **London, United Kingdom** (acceptable - explicitly open to relocation here)
- Any other city or country: too far - exclude from results

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
