# ADR 002: Scope and positioning

**Status:** Accepted | **Date:** 2026-11-24

## Context
Noura is public, non-monetized, and built with production-grade practices. A real medical service needs clinician oversight and a liability posture that this project does not have yet.

## Decision
- The Feb 28 launch is a **public prototype**, clearly labeled "informational, not medical advice, not for emergencies," not a real service.
- Scope v1: postpartum mothers. Topics are recovery, postpartum mental health, breastfeeding, and infant safe sleep. Newborn illness, vaccines, and medication questions get "I don't cover that" plus a referral.
- v1 languages and channels: English; text chat plus a browser voice demo.
- Later: real phone IVR (English and Spanish) and memory/personalization.

## Consequences
- A narrow scope keeps the eval set, abstention tests, and safety layer tractable.
- Promoting the prototype to a real service later requires clinician review of the safety layer.
- The not-for-emergencies notice appears on every screen and is spoken in the voice demo.
