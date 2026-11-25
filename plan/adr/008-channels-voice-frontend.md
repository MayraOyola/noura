# ADR 008: Channel-agnostic core, voice demo, and frontend

**Status:** Accepted | **Date:** 2026-11-24

## Context
The long-term goal is IVR where mothers type or speak in English or Spanish. A browser voice demo is in scope for the 14-week plan; real phone IVR is not.

## Decision
- The core is a **text-in, text-out service**; the channel (web, voice) is a thin adapter.
- Answers return as **structured data**: answer text, a list of citations, an escalation flag. No markdown or inline links in the core's output.
- Escalation wording lives in one place and is rendered per channel (shown in text, spoken in voice).
- The browser voice demo uses the browser's speech APIs. It needs its own emergency handling: misheard speech, dropped connections, and how Tier 1 is spoken.
- The frontend is a thin React adapter over the core. The emergency banner and escalation display are launch-gate items and get manual review before launch.

## Consequences
- Real phone IVR, with a telephony provider and speech services, becomes an adapter later rather than a rewrite.
- The voice demo adds about two weeks, so launch moved from Feb 14 to Feb 28.
