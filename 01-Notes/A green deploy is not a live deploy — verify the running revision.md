---
created: 2026-06-13
tags: engineering, reliability, deploy, ci-cd
status: evergreen
---

# A green deploy is not a live deploy — verify the running revision

> A deploy pipeline reports success when it *hands off* the new artifact, not when the new artifact is *serving*. Those are different moments, and the gap between them is where "I shipped it but it's not live" lives.

## Context
The CI/CD job builds the image, pushes it, and tells the platform to use the new tag — then exits green. But the platform brings the new revision up **asynchronously**: it health-checks the new container and only shifts traffic once it's healthy. If the new revision **crash-loops on startup**, most platforms (Azure Container Apps, k8s rollouts, ECS) keep serving the **previous healthy revision** and the deploy job *still shows success*. So the app's *template* says "new image" while the *active revision* runs old code — and your green checkmark is lying about what users hit.

## The rule
After any deploy, verify the **running revision**, not the pipeline status:
- Confirm the **newest** revision is `Healthy` / `Running` with replicas > 0 (not `ActivationFailed`).
- **Direct-curl the service** (bypass any CDN/edge) and assert a *new* behavior introduced by this deploy — not just `200 OK`, which the old revision also returns.
- If the new revision failed, the crash logs are on *that revision*, not the app in general — query by revision name.

Corollary for env/config changes: build-time config (`NEXT_PUBLIC_*`, baked secrets) does nothing until a **rebuild**, and "empty"/unrelated commits may be skipped by ignored-build-step rules — so setting a variable and seeing a green deploy is doubly not proof. Sibling failure mode: [[Liveness can't catch a wedged dependency — readiness needs a watchdog]] — there the revision is *healthy* but still not serving.

## Where this bit us (Arth Saathi)
The email-auth ship (2026-06-04) was code-complete for hours but invisible in prod: new backend routes 404'd while old routes in the same file worked, because the live Azure Container Apps revision was old code — the new one had crash-looped on startup (corepack auto-upgrading pnpm onto a Node 20 image) and traffic never moved, yet the deploy was green. The fix and the full verification runbook live in [[Backend deploy & revision health — Arth Saathi]].

## Related
- [[Backend deploy & revision health — Arth Saathi]]
- [[Liveness can't catch a wedged dependency — readiness needs a watchdog]]
- [[Arth Saathi]]
