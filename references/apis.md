# Literature Search APIs (free, no keys)

All of these work with plain HTTPS requests — `curl` or WebFetch. Respect the
rate limits; the COLLECT side searches them in parallel where allowed.

## OpenAlex
- Search: `GET https://api.openalex.org/works?search={query}&per_page=25&sort=cited_by_count:desc`
- By DOI: `GET https://api.openalex.org/works/https://doi.org/{doi}`
- Rate: ~10 req/s, parallel OK. Add `&mailto=you@example.com` for the polite pool.

## Semantic Scholar
- Search: `GET https://api.semanticscholar.org/graph/v1/paper/search?query={q}&limit=20&fields=paperId,title,authors,year,venue,citationCount,abstract,externalIds,openAccessPdf`
- Rate: 1 req/s — run these calls **serially**, never in parallel.

## PubMed (E-utilities)
- Search: `GET https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term={query}&retmax=50&retmode=json`
- Summaries: `GET https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id={id1,id2}&retmode=json`
- Abstracts: `GET https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=pubmed&id={id}&rettype=abstract&retmode=text`
- Rate: 3 req/s without an API key. Use MeSH terms in `term` for precision
  (e.g. `term=mixed models[MeSH Terms] AND clinical trials`).

## Europe PMC
- Search: `GET https://www.ebi.ac.uk/europepmc/webservices/rest/search?query={query}&format=json&pageSize=25`
- Best for: abstracts, open-access full text links, and preprints (bioRxiv /
  medRxiv are indexed here).

## Crossref (DOI verification)
- By DOI: `GET https://api.crossref.org/works/{doi}`
- Use this to verify every paper before it enters the KB: title, authors,
  year, and journal must match what the search returned. If the DOI does not
  resolve, drop the paper.

## arXiv
- Search: `GET http://export.arxiv.org/api/query?search_query=all:{query}&max_results=20&sortBy=submittedDate&sortOrder=descending`
- Best for: stats / methods preprints. Note the `http` endpoint (not https).

## Practical notes
- Deduplicate on DOI first, normalized title second.
- Record the exact query string, source, date, and hit count for every call
  in the topic's `search-log.md`.
- For a refresh run, add a date filter (e.g. OpenAlex
  `filter=from_publication_date:2026-01-01`; PubMed `term=... AND 2026/01/01:3000[dp]`).
