# ADR 003: Safety architecture (semantic router, two-tier escalation, fail closed)

**Status:** Accepted | **Date:** 2026-11-24

## Context
The agent must never diagnose or give dosing advice, and urgent warning signs must trigger an escalation response. Missing a real emergency costs far more than a false alarm. Noura is also not intended for emergencies, and a frightened mother should not be addressed in an alarming tone.

## Decision
- A **semantic router** sits in front of everything. It embeds the message and compares it to example phrases per route: emergency, mental-health crisis, diagnosis/dosing request (refuse), out of scope (abstain and refer), in scope (continue to retrieval).
- **Two-tier escalation, calm tone, no exclamation points or all-caps.**
  - Tier 1, urgent signs: calmly says this needs care right now, call 911 or 988, and shows the CDC Hear Her signs.
  - Tier 2, concerning but not urgent: suggests contacting her provider today.
- **Fail closed:** if the router errors, times out, or cannot decide, the user sees the static resources page (911/988, CDC warning signs). The message is never sent to the LLM unscreened.
- Thresholds are tuned for recall; false alarms are acceptable.
- Escalation wording is defined once and rendered per channel (text, spoken).

## Consequences
- The router depends on the embedding API, so outages mean the site shows the static page.
- The static page doubles as the kill switch.
- The urgent-sign eval set must pass at 100% before launch.

## Open details
- Example phrases per route, thresholds, and how router recall and precision are measured.
- Whether to add a small deterministic keyword check as a belt-and-braces layer (not decided).
- Exact Tier 1 and Tier 2 wording; get a nurse or clinician to review it if possible.
