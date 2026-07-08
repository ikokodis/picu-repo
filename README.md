# PICU Literature Repository

A single-file, no-build website that pulls high-evidence-tier pediatric ICU (PICU) research
directly from PubMed and organizes it by evidence tier.

## Use it

Open `index.html` in any browser — no server or build step required. It calls the NCBI
PubMed E-utilities API directly from the browser (no API key needed).

## Deploy it for one-click access

- **Netlify Drop** — go to https://app.netlify.com/drop and drag in `index.html`. You get a
  permanent URL instantly.
- **GitHub Pages** — push this repo to GitHub, enable Pages in repo settings (source: root of
  main branch), and it's served at `https://<username>.github.io/<repo>/`.
- **Local** — just double-click `index.html`, or bookmark the `file://` path.

## How it works

1. **esearch** — queries PubMed with a base PICU filter (MeSH "Intensive Care Units, Pediatric"
   or title/abstract PICU terms) + your topic + a publication-type filter for the selected
   evidence tier, restricted to your chosen time window. Low-quality types (case reports,
   comments, editorials, letters, news) are excluded at the query level.
2. **efetch** — pulls full article XML for the matched PMIDs (abstract, authors, journal,
   publication types, DOI, MeSH terms).
3. **Client-side classification** — assigns an evidence tier (1/2/3) and a 1-5 quality score
   (tier + a bonus for top PICU/pediatric/critical-care journals) purely from the PubMed
   metadata already fetched — no external AI call, so it works offline from any CORS
   restriction and never fails silently.

## Features

- Quick-topic chips for recurring searches (pCART/pCREST, ED transfer/triage, ML/informatics,
  ECMO, prediction scores, sepsis, ventilation)
- Pin your own searches for one-click re-run (persisted in `localStorage`)
- Save/bookmark articles across sessions, export saved list to CSV
- Dark mode (persisted)
- Evidence tier filter (Tier 1 only / Tier 1+2 / all high-quality) and adjustable time window
  (1-5 years)

## Notes

- Tier and quality score are computed from PubMed's own publication-type and MeSH data —
  always confirm study design against the full text before citing.
- An earlier version explored calling the Anthropic API from the browser for AI-written
  summaries; that was dropped because a static page can't call the Claude API directly
  without CORS/a backend proxy. The extractive summary (pulled from the abstract's own
  "Conclusion" section when present) is a deliberate, working replacement.
