# ADR 005: LLM provider and spend control

**Status:** Accepted | **Date:** 2026-11-24

## Context
The service is public and non-monetized, with a hard monthly budget of about $30-50. Options were the Anthropic API directly, Amazon Bedrock, or a self-hosted open model.

## Decision
- Use the **Anthropic API directly**, behind a small `LLMClient` interface so Bedrock or another provider can be swapped in.
- Use the provider's workspace spend limit as a hard cap (check the console controls before relying on it), plus an app-level **daily token budget counter** as a second cap.
- When any cap is hit, the app falls back to the static resources page.

## Consequences
- Simple setup and a real hard cap.
- AWS Budgets mostly sends alerts and does not stop spend, so it supports the cap but is not the cap.
- Self-hosted open models need GPU hosting, which breaks the budget.

## Open details
- Which model, and how it is chosen from the eval results.
- Where the token counter lives so it cannot be bypassed.
