# ADR 001: Document metadata schema

**Status:** Accepted | **Date:** 2026-11-25 (fields set 2026-11-24)

## Context
Every answer must be traceable to a source, and sources change over time. Project rules require source org, URL, retrieval date, and last-reviewed date on every ingested document. Overlapping sources (e.g. NIMH perinatal depression as HTML and PDF; four NICHD safe-sleep pages) need to be marked without deleting originals.

## Decision
Every ingested document carries these fields:
- `source_org`, `url`, `retrieval_date`, `last_reviewed_date` (project rules)
- `license_note`: the license or terms of use, recorded per document so source licensing is auditable
- `content_hash` and `doc_version`: power idempotent ingestion and prove which version an answer cited
- `topic`: tag used for canonical-document selection and retrieval filters
- `superseded_by`: points to the canonical document; superseded documents stay in the repo but are excluded from retrieval
- `language`: avoids a migration when Spanish documents arrive

## Consequences
- Ingestion can skip unchanged documents and flag stale ones.
- Citations can show org, URL, retrieval date, and last-reviewed date.
- More required fields means more work per new source, which is intended.

## Details decided (2026-11-25)

**Storage.** One `docs/manifest.json` is the single source of truth, validated with Pydantic. Original HTML and PDF files and the converted markdown stay plain content. Front matter in the markdown files is removed, so metadata cannot drift or end up inside chunks.
- *Considered:* front matter in each markdown file (originals cannot hold it); both places (can disagree); YAML (easier to hand-edit, but scripts will update the manifest in Week 4 and JSON is stricter and round-trips exactly).

**Types.**
- Dates are real ISO `YYYY-MM-DD` dates. A source that gives only month and year is stored as the first of that month, with a note that the day was unknown.
- `source_org`, `topic`, `language`, and the license value come from fixed lists, so labels stay consistent and a new value is a deliberate one-line change.

**Required and optional.**
- Required: `source_org`, `url`, `retrieval_date`, `content_hash`, `topic`, `language`, `license_note`.
- May be empty: `superseded_by`.
- `last_reviewed_date` is required but may hold an explicit `unknown`. The validator warns, and staleness checks treat the document as stale. This keeps the project rule honest without rejecting useful sources that print no date.
- `license_note` is a short fixed value plus an optional free-text note. It stays required because the project rule is conservative sourcing; it is cheap for US government sources and is the audit trail for newer ones.

**Identifiers and versions.**
- `doc_id` is the file name stem (e.g. `owh_postpartum_depression`). The validator enforces uniqueness. An ID is never renamed once it has been cited. *Considered:* UUID or URL hash (safe for large automated ingestion, but unreadable in citations and logs).
- `doc_version` is a counter that increases whenever `content_hash` changes. The hash of each version is kept so a citation can be proven.
- `content_hash` fingerprints the converted markdown only. *Considered:* also hashing the original file as `source_hash` (more coordination for now).

**Validation.**
- Errors that fail CI: missing required field, bad date format, duplicate `doc_id`, manifest entry with no file, file in `docs/` with no manifest entry.
- Warning only: `last_reviewed_date: unknown`.
- The check runs as a local command and as a CI step, so a bad manifest cannot be merged.

## Revisit if
- Ingestion becomes large or automated: reconsider UUIDs for `doc_id` and splitting the manifest.
- Converter changes cause false "changed" alarms: add `source_hash` of the original file.
- Hand-editing the manifest becomes painful: reconsider YAML.

## Follow-ups
- Save the URLs from `src/noura/convert_to_markdown.py` into the manifest, and settle the conversion before Week 2 and before any hashes are computed.
- Choose the canonical document for each overlapping topic in Week 2 and set `superseded_by` accordingly.
