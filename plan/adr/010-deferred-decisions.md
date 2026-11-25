# ADR 010: Deferred decisions register

**Status:** Open | **Date:** 2026-11-24

Decisions intentionally not made yet, each with a trigger. Replace this entry with a real ADR when the trigger arrives.

| Decision | Decide by | Notes |
|---|---|---|
| Infrastructure-as-code tool (Terraform or CDK) | Week 3 | Pick based on cost, fit with CI, and maintainability |
| AWS compute (Lambda + streaming behind CloudFront, ECS Fargate, App Runner) | End of Week 10 | Constraints: streaming responses, $30-50/month, survives abuse spikes. Check App Runner is still open to new accounts. Fargate plus a load balancer likely eats most of the budget |
| Abuse protection (rate limits, bot protection) | End of Week 10 | Must work with the spend caps |
| Memory and personalization design | After launch | See ADR 009 |
| Second vector-store backend (e.g. MongoDB) | After launch, optional | Verify free-tier vector search limits first |
| Section-level citation display | After launch, optional | See ADR 006 |
| Real phone IVR and Spanish | After launch | See ADR 008 |
