---
name: lit-review-kb
description: Two-sided literature review skill for a split environment. COLLECT (web access): search PubMed, OpenAlex, Semantic Scholar, Europe PMC, and arXiv for a topic, verify every paper by DOI, and write a structured knowledge base (KB) of papers, search log, and synthesis to a GitHub repo. ANALYZE (no web access): read only the KB from GitHub and answer analysis questions, draft analysis plans, and synthesize themes, citing KB entries only. Use when the work environment cannot reach the internet but can reach GitHub, or when a literature review must be reproducible and auditable.
---

# Literature Review KB — Two-Sided Skill

## The problem it solves

One environment has web access, the other does not — but both need the same
literature. The fix is a knowledge base (KB) on GitHub as the bridge:

```
Personal Claude (web)                Work Claude Code (no web, GitHub OK)
  COLLECT: search APIs, verify,         ANALYZE: git clone/pull the KB repo,
  screen, extract, write KB    ──▶     read only the KB, answer questions,
  push to GitHub repo                  draft analysis plans, cite KB entries
```

All KB content comes from public sources (papers, abstracts, metadata), so a
public GitHub repo is fine. No company data ever enters the KB.

## COLLECT — personal side (web access)

### Step 1. Define the question first
Before searching, write `topics/<topic>/index.md`: the research question,
PICO (Population, Intervention, Comparison, Outcome) for clinical topics,
inclusion/exclusion criteria, date range, and languages. No search happens
until this exists — it is what makes the review reproducible.

### Step 2. Search the free APIs (no keys needed)
Query in parallel where the API allows it. Exact endpoints and rate limits:
`references/apis.md`. Source selection:

| Topic type | Sources |
|---|---|
| Biomedical / pharma | PubMed + Europe PMC + OpenAlex + Semantic Scholar |
| Stats / methods (general) | OpenAlex + Semantic Scholar + arXiv + Crossref |
| Preprints | arXiv + bioRxiv (via Europe PMC) + OpenAlex |

Use several query variants per topic (synonyms, MeSH terms for PubMed).
Default: last 10 years unless the question needs older work; record the
date range in the search log.

### Step 3. Deduplicate by DOI
Merge results across sources on DOI (fall back to normalized title). Keep a
count at each stage for the PRISMA-style flow in `search-log.md`:
identified → after dedup → after title/abstract screen → included.

### Step 4. Verify before writing — no exceptions
Every paper written to the KB must resolve: check its DOI through Crossref
or OpenAlex, and confirm title/authors/year/journal match. A paper that does
not verify is dropped, never written. This is the rule that prevents
hallucinated references.

### Step 5. Screen and extract
Screen title/abstract against the inclusion criteria; record the exclusion
reason for every excluded paper (one line in `search-log.md`).
For each included paper, write `topics/<topic>/papers/<doi-slug>.md` with:
title, authors, year, journal/venue, DOI, PubMed ID, URL, abstract, study
design, population, key findings, limitations, relevance to the question,
and a confidence note. Store metadata + abstract + your own notes only —
never full copyrighted text. Open-access full-text links are fine.

### Step 6. Synthesize, then push
Write `topics/<topic>/synthesis.md`: themes (not paper-by-paper summary),
agreements and tensions between studies, and gaps. Tier every claim:
High (replicated across unflagged studies), Medium (plausible but limited by
sample, method, or a flagged weakness), Low (single-source, early, or
speculative). Then commit and push the KB repo so the work side can pull it.

## ANALYZE — work side (GitHub only, no web)

1. `git clone` (first time) or `git pull` (updates) the KB repo.
2. Read `topics/<topic>/index.md` and `synthesis.md` first, then the paper
   files as needed.
3. Answer questions and draft analysis plans using KB content only. Every
   claim cites a KB paper file and its DOI (e.g. `papers/smith-2024-mixed-models.md`).
4. If a claim is not in the KB, say "not in KB" and request a COLLECT run for
   that question. Never invent a paper, a DOI, or a finding to fill the gap.
5. Never browse the web from this side — there is no web access, and the KB
   is the single source of truth.

## Refreshing a topic

Re-run COLLECT with a date filter (e.g. papers since the last search date in
`search-log.md`). New papers get new files; the synthesis is updated, never
rewritten from scratch; the search log appends the new run.

## Rules that apply to both sides

- Retrieve first, then write. No claim without a source file behind it.
- DOIs must resolve. If it does not resolve, it does not exist.
- Abstracts and links only — no full text in the KB.
- The search log is written before the synthesis, and it records queries,
  dates, sources, and counts.
- The KB is public-source only. Company data, internal documents, and
  patient data never enter it.
