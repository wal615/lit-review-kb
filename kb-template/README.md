# Literature Knowledge Base

A structured, versioned home for literature reviews. One folder per topic
under `topics/`. Everything here comes from public sources — paper metadata,
abstracts, and links. No full text, no company data.

## How the work side uses this KB

The work environment has no web access but can reach GitHub. It clones this
repo and treats it as the single source of truth for literature:

1. `git clone <this-repo>` (first time) or `git pull` (updates).
2. Read `topics/<topic>/index.md` and `synthesis.md` first, then the paper
   files as needed.
3. Every claim in an answer or analysis plan cites a paper file and its DOI.
4. If something is not in the KB, the answer is "not in KB" — the work side
   never invents a paper. Missing coverage is a request for a new COLLECT run
   on the personal side.

## Layout

```
topics/<topic>/
├── index.md        ← research question, PICO, inclusion/exclusion criteria
├── search-log.md   ← every query: date, source, query string, hits, kept
├── synthesis.md    ← themes, agreements/tensions, gaps, confidence tiers
└── papers/
    └── <first-author-year-short-title>.md   ← one file per included paper
```

## Conventions

- One topic = one folder, named in lowercase with hyphens
  (e.g. `topics/mixed-models-missing-data/`).
- One paper = one file, named `<first-author>-<year>-<short-title>.md`.
- Every paper file carries a DOI; every DOI was verified through Crossref or
  OpenAlex before the file was written.
- Claims in `synthesis.md` are tiered: High (replicated across unflagged
  studies), Medium (plausible but limited), Low (single-source or speculative).
- The search log is written before the synthesis, and it records the exact
  queries, dates, sources, and counts at each screening stage.
