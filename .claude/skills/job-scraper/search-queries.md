# Search Queries for Job Scraper

## Search Sites

Juan is based in Madrid, Spain and open to relocation to London only. The built-in job-portal CLI tools in this repo (`jobindex-search`, `jobbank-search`, `jobdanmark-search`, `jobnet-search`) are **Danish-market-specific** and not relevant here. Use instead:

Primary:
- **linkedin.com/jobs** - via the `linkedin-search` CLI (`.agents/skills/linkedin-search/`), which is not region-locked
- Direct Google `site:` searches against LinkedIn Jobs, company career pages, and finance-specific job boards

Recommended finance-specific boards (via Google `site:` search, no dedicated CLI yet):
- **efinancialcareers.com** / **efinancialcareers.co.uk** - the leading IB/M&A job board for both Madrid and London markets
- **infojobs.net** - largest general Spanish job board (useful for Madrid-based roles)
- Company career pages directly for target banks/boutiques

If Juan wants dedicated CLI scraping for eFinancialCareers or InfoJobs (instead of Google site-searches), use `/add-portal` to generate a portal search skill for either site.

## Query Categories

Queries are grouped by priority. Each query should be combined with location terms ("Madrid" or "London") since those are the only two acceptable locations.

### Priority 1: Tech M&A Analyst / Associate

These match Juan's strongest and current career direction.

```
site:linkedin.com/jobs "Tech M&A Analyst" Madrid OR London
site:linkedin.com/jobs "M&A Associate" technology Madrid OR London
site:efinancialcareers.com "M&A" "Analyst" Madrid
site:efinancialcareers.co.uk "M&A" "Analyst" London
```

### Priority 2: Investment Banking Analyst (broader IB)

These match Juan's broader IB experience (Citi) and domain expertise in valuation/financial modelling.

```
site:linkedin.com/jobs "Investment Banking Analyst" Madrid OR London
site:efinancialcareers.com "Investment Banking Analyst" Madrid
site:efinancialcareers.co.uk "Investment Banking Analyst" London
```

### Priority 3: Corporate Development / Strategy (adjacent pivot)

Adjacent roles at tech companies that Juan could pivot into.

```
site:linkedin.com/jobs "Corporate Development" Analyst Madrid OR London
site:linkedin.com/jobs "Strategy Analyst" technology Madrid OR London
```

### Priority 4: Broader Finance / Consulting

Wider net for general finance and consulting roles that use similar skills.

```
site:linkedin.com/jobs "Financial Analyst" technology Madrid OR London
site:linkedin.com/jobs "Technical Consultant" finance Madrid OR London
site:infojobs.net "analista M&A" OR "analista financiero" Madrid
```

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
