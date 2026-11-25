# ADR 006: Citation granularity

**Status:** Accepted | **Date:** 2026-11-24

## Context
Every answer must be traceable to a source. Options were document-level links, document + section + passage, or sentence-level citations.

## Decision
- v1 shows **document-level links** (source org, document title, URL, retrieval and last-reviewed dates).
- Every chunk stores its **section heading and exact passage** in metadata, even though v1 does not display them, so section-level citations can be added later and faithfulness can be measured without re-ingesting.

## Consequences
- Simplest user-facing format; weaker traceability than OpenEvidence-style passages.
- The faithfulness eval must still check that the cited document actually supports the claim.
- Revisit after launch (optional upgrade to section-level display).
