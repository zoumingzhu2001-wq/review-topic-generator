---
name: literature-survey-generator
description: Generate a complete academic literature survey from search through final revision. Use when the user asks for a literature review, survey paper, review of recent papers, journal-specific evidence synthesis, 文献综述, 综述, or a reproducible multi-agent workflow that searches scholarly databases, builds citations, drafts a structured review, checks accuracy, and produces LaTeX, Markdown, and optionally PDF outputs.
---

# Literature Survey Generator

Produce a rigorous academic literature survey using a file-based, multi-phase workflow. The workflow must remain transparent, reproducible, and grounded in retrieved sources.

## Inputs

Extract or infer:

- topic and key concepts
- target journals, disciplines, or databases
- publication date range
- inclusion and exclusion criteria
- preferred language
- output format: LaTeX, Markdown, or both
- whether a compiled PDF is required

When the user does not specify journals, select appropriate high-quality journals or broad scholarly databases for the discipline rather than defaulting blindly to economics journals.

## Working directory

Create:

```text
agent_tasks/{topic_slug}_{YYYYMMDDHH}/
```

Write `plan.md` with the scope, search strategy, phases, expected outputs, and unresolved assumptions.

## Phase 1: Search

Run journal- or database-specific searches in parallel when possible.

Preferred metadata sources:

1. OpenAlex
2. Crossref
3. discipline-specific sources available to Codex
4. publisher or journal sites for verification

For each record capture:

- title
- authors
- journal or source
- publication year and date
- DOI or stable identifier
- abstract
- citation count when available
- open-access or PDF locations
- search source and query

Save each search stream as `search_{source}.json`. Deduplicate primarily by DOI, then normalized title.

Do not use a hard-coded third-party email address. Add `mailto` or contact-email parameters only when the user has explicitly configured one. For Unpaywall, use `UNPAYWALL_EMAIL` when available; otherwise skip Unpaywall downloads rather than inventing an address.

After searching, write a tally of records per source. If fewer than 3 relevant records remain, broaden terms or date range and document the change. If more than 100 remain, narrow the scope or introduce a screening stage.

## Phase 2: Process evidence

Run these tasks in parallel where possible.

### A. PDF and full-text retrieval

For each included paper:

- prefer legal open-access locations
- verify downloads are actual PDFs and larger than 10 KB
- save to `pdfs/{safe_filename}.pdf`
- record success, failure, URL, and reason in `pdfs/download_report.md`

Never bypass paywalls or access controls.

### B. Citation library

For DOI-bearing papers, request BibTeX from DOI or publisher metadata. Normalize citation keys to:

```text
{firstauthor}{year}{keyword}
```

Save `references.bib`. Check for duplicate keys and missing required fields.

### C. Evidence summaries

Create `all_papers.json` and `paper_summaries.md`. For every included paper record:

- bibliographic details
- research question
- study design and population or data source
- methods
- main findings
- limitations
- relevance to the review question
- evidence used for the summary: abstract only or full text

Never present abstract-only inference as though full text was reviewed.

## Phase 3: Draft

Draft `survey.tex`, `survey.md`, or both from the processed evidence files, not from memory.

Recommended structure:

1. Title
2. Abstract
3. Introduction and rationale
4. Search and selection methodology
5. Summary table of included studies
6. Thematic synthesis sections
7. Methodological comparison
8. Limitations of the evidence base and of this review
9. Research gaps and future directions
10. Conclusion

Organize the body by themes, mechanisms, methods, populations, or outcomes rather than merely listing papers one by one. Provide substantive comparison across studies. Identify disagreements and plausible sources of heterogeneity.

For LaTeX:

- use `\citet{}` for narrative citations
- use `\citep{}` for parenthetical citations
- verify every citation key exists in `references.bib`
- use the template in `references/latex-template.md`

If LaTeX tooling is available, compile with:

```bash
pdflatex -interaction=nonstopmode survey.tex
bibtex survey
pdflatex -interaction=nonstopmode survey.tex
pdflatex -interaction=nonstopmode survey.tex
```

Fix compilation errors before review.

## Phase 4: Review and revise

Create `review_report.md` that evaluates:

- factual consistency with abstracts or full text
- appropriateness of study grouping
- completeness of coverage
- citation correctness
- academic writing quality
- unsupported claims
- treatment of conflicting findings
- at least 5 specific revisions with HIGH, MEDIUM, or LOW priority
- overall score out of 10

Then create `survey_v2.tex` and/or `survey_v2.md`, implementing all justified revisions. Recompile the revised LaTeX document when tooling is available.

## Delivery

Report:

- exact search scope and dates
- records found, deduplicated, screened, and included
- papers found per source or journal
- full-text retrieval success rate
- whether each paper was summarized from abstract or full text
- review score and major revisions
- final file paths

List all deliverables in the working directory.

## Critical rules

1. Use file handoff between phases and subagents; do not rely on large context handoffs.
2. Parallelize independent searches and processing, but keep search → process → draft → review → revision sequential.
3. Preserve an auditable search log, including queries, dates, filters, and screening reasons.
4. Verify source metadata, DOI, year, and journal before citing.
5. Never fabricate papers, citations, abstracts, findings, page counts, or successful PDF downloads.
6. Clearly label tangentially relevant papers and abstract-only evidence.
7. Respect API rate limits, robots rules, copyright, and access controls.
8. A polished narrative is not a substitute for systematic screening or source verification.
