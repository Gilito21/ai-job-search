# Job Application Assistant for Juan Peláez Echániz

## Role
This repo is a job application workspace. Claude acts as a career advisor and application assistant for Juan Peláez Echániz, helping with:
1. **Job fit evaluation** - Assess job postings against your profile (skills, experience, behavioral traits)
2. **CV tailoring** - Adapt existing CV templates (LaTeX/moderncv) to target specific roles
3. **Cover letter writing** - Draft targeted cover letters using existing templates (LaTeX)
4. **Interview preparation** - Prepare answers, questions, and talking points for interviews
5. **Career strategy** - Advise on positioning and personal branding

## Candidate Profile

### Identity
- **Name:** Juan Peláez Echániz
- **Location:** Madrid, Spain (open to relocation - Madrid and London only)
- **Languages:** Spanish (Native), English (Proficient, Cambridge Advanced Certificate C1), French (Basic)
- **Status:** Employed (Analyst, Tech M&A at BlueBull Partners), open to new opportunities
- **LinkedIn headline:** "Analyst (Tech M&A) at BlueBull Partners"

### Education
- **Dual Bachelor in Business Administration and Data & Business Analytics** (2020-2025) - IE Business School, Madrid and Segovia, Spain
  - Honors (Top 5%): Mathematics, Algorithms, Blockchain Technologies
- **Exchange Program in Business Administration** (Aug-Dec 2023) - UNC Kenan-Flagler Business School, North Carolina, USA

### Professional Experience
- **Analyst (Tech M&A)** (January 2026 - Present) - **BlueBull Partners** (Madrid, Spain)
  - Advised on 3 mandates simultaneously (sell-side and buy-side), combined EV over €300M
  - Conducted in-depth research across EdTech, B2B SaaS and IT Services verticals
  - Leveraged Claude and other AI tools to accelerate deal research, financial modelling and pitch material, standardising deliverables and cutting analysis turnaround
- **Junior Analyst (Tech M&A)** (January 2025 - December 2025) - **BlueBull Partners** (Madrid, Spain)
  - Supported companies with over €5M EBITDA in M&A, debt structuring and equity fundraising
  - Contributed to 10+ projects with pitch decks, financial models and industry analysis
- **Spring Insight Analyst (Investment Banking)** (April 2023) - **Citi** (London, United Kingdom)
  - Developed competency in company valuation, financial strategy and market analysis
  - Built relationships with senior bankers across divisions, gaining first-hand exposure to the sector

### Technical Skills
- **Primary:** Python, SQL, financial modelling, valuation, M&A deal execution
- **Secondary:** Spark, R, Supabase, PostgreSQL
- **Domain:** Tech M&A (EdTech, B2B SaaS, IT Services), investment banking, sell-side/buy-side advisory
- **Software:** Advanced Excel, PowerPoint, AI tools (Claude, ChatGPT, LLM-based research and modelling workflows), financial databases and tools

### Certifications
- None on file yet

### Publications
- None

### Awards
- Silver Medal, Junior Mathematics Olympiad (UK)
- Represented Everest School in Mathematics competitions (Madrid)

### Behavioral Profile
- **Analytical/quantitative strength** - strong in modelling, structured analysis, and quantitative reasoning
- **Relationship-oriented** - builds rapport easily with senior stakeholders (bankers, founders, cross-institution teams)
- **Strengths:** Quantitative rigor, financial modelling, deal research, applying AI tools to accelerate workflows
- **Growth areas:** Deepening communication/networking depth to match analytical strength
- **Thrives in:** Fast-paced deal environments; collaborative team settings

### What Excites You
- The tech/deal content itself - researching companies, sectors, and what makes tech businesses valuable
- Using AI and LLM-based tools (especially Claude) to modernize traditionally manual finance workflows
- Building relationships with senior bankers, founders, and other stakeholders

### Target Sectors
- Tech M&A / Investment Banking: BlueBull Partners, Citi, and comparable bulge-bracket/boutique advisory firms in Madrid and London
- Adjacent: Corporate Development / Strategy roles at tech companies

### Deal-breakers
- Open to relocation between Madrid and London specifically (not open to other geographies at this time)
- **No existing UK right to work** - any London/UK role requires the employer to sponsor a visa; roles that state they will not sponsor are a hard exclude
- **No M&A/legal roles** - excludes litigation finance and similar legal-adjacent M&A work (e.g. Burford Capital-style firms)

## Repo Structure
- `cv/` - LaTeX CV variants (moderncv template, banking style)
- `cover_letters/` - LaTeX cover letters (custom cover.cls template)
- `.claude/skills/` - AI skill definitions for the application workflow
- `.agents/skills/` - Job search CLI tools

## Workflow for New Job Applications
1. User provides a job posting (URL or text)
2. **Always evaluate fit first**: skills match, experience match, behavioral/culture match. Present this assessment to the user before proceeding.
3. If good fit: create targeted CV (`cv/main_<company>.tex`) and cover letter (`cover_letters/cover_<company>_<role>.tex`)
4. **Verify both documents** (see Verification Checklist below)
5. Prepare interview talking points based on the role requirements and your strengths

**Important:** When mentioning agentic coding or AI tooling in CVs/cover letters, explicitly reference **Claude Code** by name.

## Verification Checklist
After creating or updating a CV or cover letter, re-read the generated file and verify **all** of the following before presenting to the user. Report the results as a pass/fail checklist.

### Factual accuracy
- [ ] All claims match actual profile (CLAUDE.md / candidate profile) - no fabricated skills, experience, or achievements
- [ ] Job titles, dates, company names, and locations are correct
- [ ] Contact details are correct
- [ ] All company-specific claims (partnerships, products, technology, expansions) have been independently verified via WebFetch/WebSearch - do not trust reviewer agent research without verification

### Targeting
- [ ] Profile statement / opening paragraph is tailored to the specific role (not generic)
- [ ] Skills and experience bullets are reframed to match the job requirements
- [ ] Key job requirements are addressed (with gaps acknowledged where relevant)
- [ ] Nice-to-have requirements are highlighted where there is a match

### Consistency
- [ ] CV follows the standard 2-page moderncv/banking format
- [ ] Cover letter uses cover.cls template and established structure
- [ ] Tone is consistent across CV and cover letter
- [ ] No contradictions between CV and cover letter content

### Quality
- [ ] No LaTeX syntax errors (balanced braces, correct commands)
- [ ] No spelling or grammar errors
- [ ] Agentic coding / AI tooling references mention **Claude Code** by name
- [ ] Cover letter is addressed to the correct person (or "Dear Hiring Manager" if unknown)
- [ ] Cover letter fits approximately one page

### Compiled PDF verification (MANDATORY - never skip)
Both documents MUST be compiled and visually inspected via the Read tool on the PDF output. "Looks fine in the .tex" is not acceptable - LaTeX page-break decisions are unpredictable. Iterate until these all pass:
- [ ] CV compiled with **lualatex** (pdflatex often fails on modern MiKTeX with fontawesome5 font-expansion errors). Cover letter compiled with **xelatex** (cover.cls requires fontspec).
- [ ] **CV is exactly 2 pages** - not 1, not 3
- [ ] **No orphaned `\cventry` titles** - a job/education title must never sit at the bottom of a page with its bullets spilling to the next page. Use `\needspace{5\baselineskip}` before each `\cventry` to prevent this, and `\enlargethispage{2-3\baselineskip}` to rescue a trailing section that just barely spills
- [ ] **Cover letter is exactly 1 page** - signature block must fit with the body, never overflow
- [ ] **Cover letter bullet font matches body font** - `\lettercontent{}` must not wrap `\begin{itemize}...\end{itemize}` (the command's trailing `\\` errors on `\end{itemize}`, and moving itemize outside loses the Raleway font). Standard pattern: close `\lettercontent{}`, then wrap the list in `{\raggedright\fontspec[Path = OpenFonts/fonts/raleway/]{Raleway-Medium}\fontsize{11pt}{13pt}\selectfont \begin{itemize}...\end{itemize}\par}`

### ATS & keyword verification (CV)
ATS parsers read the PDF's embedded text layer, not the rendered page. Extract it with `pdftotext -layout` and verify what a parser sees. `pdftotext` (poppler) is optional - if missing, skip the parseability items with a warning and check keyword coverage from the visual PDF read instead.
- [ ] CV text layer extracts cleanly - no `(cid:*)` markers, `�` replacement characters, or text visible in the PDF but absent from the extraction
- [ ] Email and phone appear as **literal text** in the extraction (icon-glyph noise like `MOBILE-ALT`/`Envelope` is harmless, but a contact detail carried only by an icon or hyperlink is invisible to ATS)
- [ ] Reading order of the extracted text matches the visual order (single-column stock template is safe; multi-column custom templates are where this breaks)
- [ ] Posting keywords covered or honestly absent - synonym-only matches tightened to the posting's exact term where truthfully applicable, keywords the profile genuinely supports added to experience bullets, genuine gaps left visible and **never stuffed**
