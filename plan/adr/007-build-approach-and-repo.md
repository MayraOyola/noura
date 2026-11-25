# ADR 007: Build approach and repo layout

**Status:** Accepted | **Date:** 2026-11-24

## Context
The core retrieval path in a safety-sensitive service should be simple, transparent, and testable, while commodity pieces should reuse well-maintained libraries.

## Decision
- **Hand-roll the core RAG loop** (chunk, embed, retrieve, prompt, cite) in plain Python. Reuse libraries for everything around it (FastAPI, pydantic, boto3, vector-store client, embedding SDK, pytest). No RAG framework unless a real blocker appears.
- Repo layout: `src/noura/` is the only code root (imported as `noura`), `tests/`, `docs/` for source documents, `plan/` for the roadmap and ADRs, all lowercase.
- `.gitignore` covers `.DS_Store`, `.env` and other secrets, and caches. The stray `code/` folder is removed.
- Eval questions: an LLM drafts about 80, which are cut to 40-60 with category labels and gold sources, and a mother or nurse sanity-checks a sample.
- Overlapping sources: keep all files, choose a canonical document per topic, mark the rest `superseded_by`.
- Logs hold metadata only, never message content.

## Consequences
- More code to maintain than a framework, but every step is visible and testable.
- Tooling expects the `src/` layout, so packaging and tests behave predictably.
