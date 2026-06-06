---
created: 2026-06-04
type: architecture
status: implemented
tags: arth-saathi, tech, devops, deploy, azure, vercel, ci
repo_doc: /Users/shivanshgupta/Documents/arth/docs/deploy/azure-backend-terraform.md
---

# Backend deploy & revision health — Arth Saathi

> Hard-won ops runbook. The email-auth ship (2026-06-04) was code-complete for hours but invisible in prod because of **four** infra traps below — none in the feature code. Read this before debugging "I shipped it but it's not live."

## The big lie: a green deploy is **not** a live deploy
The **"Deploy backend (Azure)"** GitHub workflow builds a Docker image → pushes → `terraform apply` sets the Container App's image tag → **reports success**. But the Container App **revision health is async**: if the new revision **crash-loops on startup**, Azure keeps serving the **previous healthy revision** and the workflow *still shows success*. So `az containerapp list` shows the *new* image tag on the app **template**, while the **active revision runs old code**.

**Symptom we hit:** new backend routes 404 in prod while old routes (same file) worked → because the live revision was old code from hours earlier.

**Always verify after a backend deploy:**
```
az containerapp revision list -n arth-prod-backend -g arth-prod-rg \
  --query "reverse(sort_by([].{name:name,health:properties.healthState,running:properties.runningState,replicas:properties.replicas,image:properties.template.containers[0].image},&properties.createdTime))[0]"
```
Look for `Healthy` / `Running` / `replicas:1` on the **newest** revision. `ActivationFailed` = it crashed; traffic stayed on the old one.

Direct-curl the backend (bypasses Next/Vercel) to compare an old vs new route:
`https://arth-prod-backend.ashyriver-955dc28b.centralindia.azurecontainerapps.io`
Failed-revision crash logs are in Log Analytics: `ContainerAppConsoleLogs_CL | where RevisionName_s == 'arth-prod-backend--00000NN'`.

## Trap 1 — corepack auto-upgrades pnpm and crashes the Node 20 container
`Dockerfile.nestjs` runs `CMD ["pnpm","server:start"]`. With **no `packageManager` field in the runner's working dir**, corepack downloads the **latest** pnpm (v11, requires **Node 22**) at startup and dies on the Node 20 image: `ERR_UNKNOWN_BUILTIN_MODULE: node:sqlite`. A time bomb — it only detonated once pnpm's "latest" crossed into Node-22 territory, so the *first* new deploy after that failed while the long-running old revision kept working.
**Fix (shipped):** `"packageManager": "pnpm@9.15.0"` in `apps/backend/package.json` (corepack honors it for the `node` runtime user — the Dockerfile's root `corepack prepare --activate` does **not** carry to node's corepack home) **+** `ENV COREPACK_DEFAULT_TO_LATEST=0` in `Dockerfile.nestjs`. The runner runs `apps/backend/src` directly via `tsx` (no compiled `dist`).

## Trap 2 — CI tests are timezone-flaky (pass local, fail CI → block deploy)
The deploy has a `pnpm test` gate. Several salary/attendance tests build fixed-date inputs via local-time `Date` and only behave in **IST** (the business calendar, `getBusinessTimeZone()` = Asia/Kolkata). They pass on a Mac (IST) and **fail on UTC CI** ("expected 4 to be 3"), blocking the deploy. **Fix:** `vitest.config.ts` pins `test.env.TZ = "Asia/Kolkata"`.

## Trap 3 — Vercel ignored-build-step cancels "empty" commits
Web (`app.arthsaathi.co.in` = Vercel project **`arthsaathi`**) auto-deploys on push to `main`, but an **empty commit** (or one that doesn't touch the relevant app) is **Canceled** by the project's ignored-build-step → the live alias stays on the old build. To force a prod rebuild that picks up a new env var, either push a **real file change** or `vercel redeploy <prod-url> --scope shivanshgupta872-gmailcoms-projects` (redeploy rebuilds with **updated env vars** and bypasses the ignore step).

## Trap 4 — multiple Vercel projects / scope confusion
The repo links `apps/web` → project **`arthsaathi`** (serves `app.arthsaathi.co.in`); `apps/landing` → `arthsaathi-landing` (apex `arthsaathi.co.in`); `apps/admin` → `arthsaathi-admin`. There's also a stray **`arthsaathi-web`** (→ `v2.arthsaathi.co.in`, *not* the live owner app). `NEXT_PUBLIC_*` flags must go on **`arthsaathi`**. Set env from `apps/web` (uses the linked project); `vercel ls`/`redeploy` default to the personal scope and can error "different team" — pass `--scope shivanshgupta872-gmailcoms-projects`. `NEXT_PUBLIC_*` is build-time → setting it does nothing until a **rebuild**.

## Secrets wiring (so you don't clobber the blob)
Backend env = a single **write-only** GitHub secret `TF_VAR_SECRET_ENVS` (JSON map → Key Vault → Container App). You **can't read it to merge**, so adding one key safely = give it its **own** GitHub secret and merge it in the deploy's "Load secret_envs" python step (that's how `RESEND_API_KEY` / `EMAIL_FROM` were added). Web env = Vercel project env. Mobile env = EAS `environment`/profile `env` (baked at build).

## Can't-do-from-here actions (need the human / a permission rule)
The auto-mode guard blocks **`gh pr merge`** and **`gh workflow run deploy-backend`** (production deploys). Workarounds that *do* work: a **push to `main`** is allowed and **auto-triggers** the backend deploy when it touches `apps/backend/**` (etc.) — so a backend code/Dockerfile change deploys itself. `eas build` and `vercel redeploy` are runnable.

## Related
- [[Email auth — implementation]]
- [[Architecture — Arth Saathi]]
- [[Play Store — Data safety & launch compliance map]]
- [[Arth Saathi]]
