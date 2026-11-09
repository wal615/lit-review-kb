# Literature Review KB Skill

A two-sided literature review skill for a split environment: one Claude has
web access, the other does not — but both need the same literature.

## The architecture

```
Personal Claude (web access)                Work Claude Code (no web, GitHub OK)
  COLLECT: search PubMed, OpenAlex,           ANALYZE: git clone / pull the KB repo,
  Semantic Scholar, Europe PMC, arXiv;        read only the KB, answer questions,
  verify every DOI; screen, extract,          draft analysis plans, cite KB entries
  write KB; push to GitHub repo
```

The knowledge base (KB) on GitHub is the bridge. Everything in it comes from
public sources — paper metadata, abstracts, and links — so a public repo is
fine. No company data ever enters the KB.

## What's inside

- `SKILL.md` — the skill: COLLECT and ANALYZE workflows, the API table, the
  verification rules, and the synthesis standard.
- `references/apis.md` — exact endpoints and rate limits for OpenAlex,
  Semantic Scholar, PubMed E-utilities, Europe PMC, Crossref, and arXiv.
- `kb-template/` — the KB structure to copy into a repo:
  - `README.md` — how the KB works and how the work side uses it
  - `topics/_template/` — one folder per topic: `index.md` (question, PICO,
    criteria), `search-log.md` (queries, dates, counts), `synthesis.md`
    (themes, confidence tiers, gaps), `papers/` (one file per paper)

## Use it as a skill

Copy this folder to `~/.claude/skills/lit-review-kb/` (all projects) or to a
project's `.claude/skills/lit-review-kb/`, then invoke it with
`/lit-review-kb` or let Claude Code pick it up when the task matches.

## Set up the KB repo

1. Create a repo (e.g. `lit-review-kb`) and copy `kb-template/` into it.
2. Personal side: give a topic (question + PICO + inclusion criteria) and
   say "collect". The KB is built and pushed.
3. Work side: `git clone <kb-repo>`, then ask questions against it — the
   skill's ANALYZE side answers from the KB only, citing paper files by DOI.

## Why not just use an existing skill?

Existing literature-review skills are good collectors — see the notes below —
but none of them is built for a split environment with an offline work side.
This skill adopts their collection methods and adds the GitHub KB bridge,
the verify-before-write rule, and the ANALYZE side that treats the KB as its
only source of truth.

Related work worth borrowing from:

- **PaperSkills / paper-orchestra** (`andchamorro/PaperSkills`) — parallel
  multi-API search (Semantic Scholar, OpenAlex, PubMed, arXiv), DOI dedup,
  citation-count ranking, verified citation registry.
- **litreview-agent** (`ckt520728/claude-skills`) — research-profile driven
  review (topic, keywords, criteria, benchmarks) with a CLI engine and REST
  API fallback.
- **medsci-skills** (`Aperivue/medsci-skills`) — `search-lit` searches
  PubMed, Semantic Scholar, and bioRxiv and verifies every citation;
  `meta-analysis` is an 8-phase systematic review pipeline.
- **Elicit skill** (`elicit/api-examples`) — search 125M+ papers and clinical
  trials, systematic reviews with evidence tables (needs an Elicit API key).
- **openscience** (`zzgw/openscience`) — fixed-budget retrieval loop over
  OpenAlex, arXiv, Crossref, PubMed, bioRxiv with claim-level links.

## Rules (short version)

- Retrieve first, then write. No claim without a source file behind it.
- Every DOI must resolve through Crossref or OpenAlex, or the paper is dropped.
- Abstracts and links only — never full copyrighted text in the KB.
- The search log is written before the synthesis.
- Work side: if it is not in the KB, say "not in KB" and request a
  collection run. Never invent a paper.

## License

MIT — free to use and adapt.
