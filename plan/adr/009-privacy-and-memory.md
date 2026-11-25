# ADR 009: Privacy in v1 and the memory roadmap

**Status:** Accepted for v1; memory design deferred | **Date:** 2026-11-24

## Context
Postpartum questions are sensitive. Remembering details such as delivery type or mental-health history is health information tied to a person. The long-term goal includes short- and long-term memory for personalization (Redis agent memory).

## Decision
- **v1 is stateless.** No message content is stored or logged. Logs hold metadata only: timestamp, latency, token counts, retrieval scores, and whether escalation fired.
- Build a small `MemoryStore` interface with a no-op implementation, so a real memory backend can plug in later without changing the chat flow.
- Memory is a **post-launch phase** (or sooner if far ahead), and starts with its own design review: what is remembered, consent, retention, deletion, and whether users are anonymous or have accounts.
- Any feedback feature stores only a rating and the retrieved document IDs, never the message text.

## Consequences
- Keeps v1 out of health-data handling.
- Personalization waits, which is an acceptable trade for a safer launch.
